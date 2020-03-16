# GitHub Actions for pbb

I used working on [Pandoc Bash Blog][1] as an excuse to look into [GitHub
Actions][2].  Eventually, I want pbb to be an action that converts your
Markdown files to HTML and deploys them to GitHub Pages instead of you having
to deploy manually.

I ended up creating three pretty simple actions.

[1]: https://github.com/bewuethr/pandoc-bash-blog
[2]: https://github.com/features/actions

## A Markdown linter

There were already actions similar to this one on the Marketplace, but a) I
wanted to learn to write actions myself and b) none of them allows to use your
own style file.

My Markdown linter of choice is [Markdownlint][3] (mdl) in its Ruby
incarnation. I've used it for a long time with [ALE][4] and have [my own style
file][5] to enforce exactly the rules I want. (Two, actually; one is
specifically for VimWiki.)

There are two types of actions: JavaScript and Docker based ones. I'm using
shell scripts in Docker containers for all my actions so far. The Docker
container and the script are both very simple; the Docker container is just
based on an Alpine image with Ruby, and I install the `mdl` gem.

The script then checks if a style file is provided as a parameter. If yes, it
runs mdl using that parameter, and if no, without it. Mdl checks all Markdown
files recursively when called with a directory as its argument:

```sh
stylefile=$1

if [ -n "$stylefile" ]; then
    mdl --style "$GITHUB_WORKSPACE/$stylefile" .
else
    mdl .
fi
```

That's all! Using the action is pretty simple:

```yml
uses: 'bewuethr/mdl-action@v1'
with:
  style-file: '.github/workflows/style.rb'
```

And without a style file, it's just a one-liner. I like getting my own style
file in a separate step in the same job:

```yml
run: curl "$STYLE_FILE" > .github/workflows/style.rb
```

where `$STYLE_FILE` is the URL of my style file on GitHub.

The action is alive and well on the [Marketplace][6].

[3]: https://github.com/markdownlint/markdownlint
[4]: https://github.com/dense-analysis/ale
[5]: https://github.com/bewuethr/dotfiles/blob/master/.config/mdl/style.rb
[6]: https://github.com/marketplace/actions/run-mdl

## A shell linter

The next action is even easier to use; it runs [ShellCheck][7] on all shell
scripts in the repository.

The Docker container uses the latest ShellCheck Alpine image and installs the
[`file`][8] command. ShellCheck doesn't find shell script files for you, so we
have to do that ourselves; the script being run in the action boils down to a
single invocation of `find`.

```sh
find . \
    -type d \
    -name '.git' \
    -prune \
    -o \
    -type f \
    -exec sh -c 'file --brief "$1" | grep -qw "shell script"' _ {} \; \
    -exec shellcheck --color=always {} +
```

First, we ignore the `.git` directory; then, for each file, we compare the
output of `file` to `shell script`, and the files that match get sent to
`shellcheck`. `--color=always` is a nice touch for the output in the GitHub
Actions tab.

I'm very used to Bash and GNU Coreutils, so when using Alpine with its BusyBox
provided ash shell and commands, I always have to double check I'm not using
some Bashism or GNUism.

Using the action requires just this:

```yml
uses: 'bewuethr/shellcheck-action@v1'
```

Like the Markdownlint action, the ShellCheck action is published on the
[Marketplace][9].

[7]: https://github.com/koalaman/shellcheck
[8]: https://www.darwinsys.com/file/
[9]: https://github.com/marketplace/actions/run-shellcheck

## A release tag prefix matcher

Actions provided by GitHub itself such as the [Checkout][10] action allow you
to specify the version you want with just a prefix:

```yml
- uses: actions/checkout@v2
```

gets you the latest release with prefix `v2`. This is a convenient way to
provide users of the action with non-breaking updates and patches without them
having the do anything, so I wanted to do the same for my actions.

As far as I can tell, these tags are manually updated, but wouldn't that be a
great use case for... an action? Exactly.

Enter the [Release tag tracker][11] action!

It goes through all tags in a repository and finds the most recent one per
minor and major release. Then, it checks if a prefix tag for them exists and if
so, deletes it; finally, it creates a new prefix tag pointing at the proper
object and pushes everything to the remote.

I got the fundamentals right pretty soon, and deleting tags worked fine.
Pushing new tags, however, eluded me for many hours. I wasn't trying to use the
GitHub API, I just wanted the action to run a shell script that pushes the new
tags in the end with a simple `git push`.

This was met with an extremely annoying error:

```txt
refusing to allow a bot to create or update workflow
```

even though I didn't do that! I tried everything I could think of or find
online:

- Use SSH instead of HTTPS for the remote URL
- Use the GitHub token explicitly in the remote URL
- Play around with appending `.git` to the remote URL

I eventually [asked in the Community Forum][12], and the (not very satisfying)
answer was that it was "because `GITHUB_TOKEN` is used to update the tag".
Deleting tags is okay, but pushing new ones isn't? Strange. And the error
message is just completely wrong, as I wasn't touching the workflow file.

In any case, the choice was now between using a personal access token or using
the API. PAT seemed to be annoying (and would trigger the action to run again,
causing an infinite loop), so API it was.

[10]: https://github.com/marketplace/actions/checkout
[11]: https://github.com/marketplace/actions/release-tag-tracker
[12]: https://github.community/t5/GitHub-Actions/quot-refusing-to-allow-a-bot-to-create-or-update-workflow-quot/m-p/49223

### API struggles

This added a few dependencies to the container: `curl` and `jq` had to be
added. I continued to struggle for a few days; the worst bug to track down must
have been the one where I kept getting "422 Unprocessable Entity" responses
from the API, but the exact same commands worked just fine when running
locally.

In the end, I found the culprit; to create the timestamp to send in the request
body, I used

```sh
date -Iseconds
```

to get an ISO 8601 timestamp with second precision. Something like this:

```txt
2020-03-15T23:36:03-04:00
```

Just as required in the [API docs][13]: "a timestamp in ISO 8601 format". Even
with a helpful Wikipedia link and a formatting template:
`YYYY-MM-DDTHH:MM:SSZ`. Aha.

After lots of squinting, I figured it out: in BusyBox, the output of `date
-Iseconds` looks like

```txt
2020-03-16T03:40:51+0000
```

First of all, UTC, but that's fine; the crucial difference is that the timezone
offset has no colon in it! [ISO 8601][14] says

> The UTC offset is appended to the time in the same way that `Z` was above, in
> the form `±[hh]:[mm]`, `±[hh][mm]`, or `±[hh]`.

And in the relevant [RFC 3339][15], "Date and Time on the Internet:
Timestamps", the formal definition of the offset is

```txt
time-numoffset = ("+" / "-") time-hour [[":"] time-minute]
```

Again, colon optional! Looks like GitHub is too strict here.

Anyway, after I finally realized all this, I also noticed that the timestamp is
optional, and I just stopped sending it. Success!

The three actions are now happily running on each other, and pbb is using them
as well.

[13]: https://developer.github.com/v3/git/tags/#create-a-tag-object
[14]: https://en.wikipedia.org/wiki/ISO_8601#Time_offsets_from_UTC
[15]: https://tools.ietf.org/html/rfc3339#page-12

## Learnings

I have a pretty good understanding of GitHub Actions now, not least because of
the quite readable [documentation][16]. I'm looking forward to playing around
with something more substantial, like turning pbb into an action.

Because my simplistic approach ("just a shell script") didn't pan out, I also
got to touch the GitHub API, which doesn't hurt either.

We tried using actions at work to run a simple pull request title linter and
even talked about moving our whole CI/CD pipeline from Jenkins to actions, but
bumped into a pretty big roadblock: a pull request from the fork of a private
repository [does not trigger a workflow][17]. Which is exactly our setup at
work.

We're [not the only ones][18] surprised by this and the [official GitHub
response][19] is

> This is a feature we want to support and plan to.

but that was three weeks ago. Until then, I'll just play around with actions
for my hobby projects, but it would be nice to get that feature eventually, as
overall, actions integrate very nicely and are quite powerful!

After this little tangent into CI/CD, I'll get back to feature work on pbb. Or
maybe some unit tests first, which would of course be run in a workflow.

[16]: https://help.github.com/en/actions
[17]: https://help.github.com/en/actions/reference/events-that-trigger-workflows#pull-request-events-for-forked-repositories
[18]: https://github.community/t5/GitHub-Actions/Will-GitHub-Actions-support-pull-request-events-from-a-fork-to-a/td-p/44488
[19]: https://github.community/t5/GitHub-Actions/Will-GitHub-Actions-support-pull-request-events-from-a-fork-to-a/m-p/47975/highlight/true#M7029
