# Bats tests for Pandoc Bash Blog

Some people say that when you think you should add tests to your Bash script,
it's really a sign that you should switch to a "proper" language instead.

Well, since "Bash" is baked into the name of this thing, that's clearly not an
option, so hello, tests!

I'm aware of a few Bash testing frameworks, but the only one I've ever used is
[Bats]; specifically, the actively maintained Bats-core fork of it. The
original project has been dormant for over four years now, but Bats-core is
alive and kicking.

I've written tests covering pretty much everything and run them in a GitHub
Actions [job], which was a bit annoying to set up, but I've learned a few
things along the way.

  [Bats]: https://github.com/bats-core/bats-core
  [job]: https://github.com/bewuethr/pandoc-bash-blog/blob/0f0014605bf48ca20641b5095b88a6b43338a200/.github/workflows/shlinttest.yml#L22-L45

## Things I've learned

Speaking of things I've learned: in writing more lines of test code than the
script they are testing has, I have bumped into a few obstacles and learned a
few new things while getting over them.

### Write testable code

This one I knew already. It's nice to have a tool that becomes interactive when
the user doesn't supply all the required parameters, but it's messy to test. I
could have made pbb prompt the user for a title if they use `pbb title` without
another parameter, but I didn't; now it's easy to test. I've always wanted to
learn [Expect], which would allow me to test these things interactively, but
until I do, I make them non-interactive.

  [Expect]: https://core.tcl-lang.org/expect/index

### Output for debugging

By default, Bats is silent and only prints output if a test fails, together
with what exactly failed. For example, I'd check if a file contains a certain
string with

```sh
grep -Fqx 'goatcountercode=mycode' .pbbconfig
```

If that fails, the output looks like

```txt
not ok 2 Set initial GoatCounter code
# (in test file gccode.bats, line 23)
#   `grep -Fqx 'goatcountercode=mycode' .pbbconfig' failed
```

but that wouldn't tell me what was in the file instead. If I just print the
file contents first, that output will show up if the test fails:

```sh
cat .pbbconfig
grep -Fqx 'goatcountercode=mycode' .pbbconfig
```

gets me this test output:

```txt
not ok 2 Set initial GoatCounter code
# (in test file gccode.bats, line 24)
#   `grep -Fqx 'goatcountercode=mycode' .pbbconfig' failed

<snip>

# blogtitle=Testblog
# goatcountercode=notmycode
```

A-ha! That makes it easier to track things down.

Want to check what files *are* there in case your existence check fails? Just
print them first:

```sh
ls artifacts
[[ -f artifacts/index.html ]]
```

### Setup and teardown functions

Bats recognizes two special functions, [`setup` and `teardown`], which are run
before and after each test. For my tests, I used them to set up a fresh temp
directory and initialize all the things I need in there: a Git repo, helper
files for pbb, and so on. Bats provides a few [variables] that come in handy to
copy files from wherever the test files resides: `$BATS_TEST_DIRNAME` is the
path to the directory containing the test, for example.

These functions can be declared in a separate file and made available to a test
file using the [`load`] directive; all of my test files just start with

```sh
load test_helper
```

and setup and teardown are taken care of.

  [`setup` and `teardown`]: https://github.com/bats-core/bats-core#setup-and-teardown-pre--and-post-test-hooks
  [variables]: https://github.com/bats-core/bats-core#special-variables
  [`load`]: https://github.com/bats-core/bats-core#load-share-common-code

### Testing Git with pushes to a remote

Some of the pbb subcommands push to a remote Git repository. I was surprised to
find how easy it is to mock that: initialize a bare repository in another
directory and tell Git that that's the remote! I use this in my `setup`
function:

```sh
# Set up "remote" repo
local remote='/tmp/pbb-remote.git'
git init --quiet --bare "$remote"

# Set up local repo
local repo='/tmp/pbb-testdata'
mkdir -p "$repo"
git -C "$repo" init --quiet

# Tell local repo about remote
git -C "$repo" remote add origin "$remote"
```

And now I can push as my heart desires!

### Bats and GitHub Actions

After diving into [Actions] a while ago and writing a few of my own, I
obviously wanted to run my shiny new Bats tests automatically in a GitHub
workflow as well.

There is an [existing action] to help set up Bats in a workflow, but it seemed
pretty easy to do it directly. I ended up with this job:

```yml
test:
  runs-on: 'ubuntu-latest'
  steps:

    - name: 'Check out code'
      uses: 'actions/checkout@v2'

    - name: 'Get Bats repository'
      uses: 'actions/checkout@v2'
      with:
        repository: 'bats-core/bats-core'
        ref: 'v1.1.0'
        path: 'bats-core'

    - name: 'Install Bats and dependencies, adjust PATH'
      run: |
        sudo apt-get install pandoc
        cd bats-core
        ./install.sh "$GITHUB_WORKSPACE"
        echo "::add-path::$GITHUB_WORKSPACE/bin"
        echo "::add-path::$GITHUB_WORKSPACE"

    - name: 'Run tests'
      run: bats --tap  test
```

This first checks out my code, and then the Bats code into a separate
directory. The third and most complex step installs Bats and then *adds it to
the `$PATH`*. This eluded me for a long time with cryptic error messages until
I finally got it.

I also learned that if a Bats test fails with exit code 127, it's because a
dependency is missing. To allow Bats to run pbb itself, I had to add the
`$GITHUB_WORKSPACE` directory, which contains the `pbb` script, to the `$PATH`,
and even though pbb doesn't shy away from using whatever tool I like, all I
need is pre-installed on the [GitHub-hosted runners].

And finally, I had to use TAP-compliant output instead of the pretty colourized
output I'm used to from interactive Bats usage; the runner doesn't like the
terminal escapes Bats uses and I didn't feel like investigating. TAP-compliant
output also says more clearly "CI", so that's fine.

  [Actions]: https://www.benjaminwuethrich.dev/2020-03-16-github-actions-pbb.html
  [existing action]: https://github.com/marketplace/actions/setup-bats-testing-framework
  [GitHub-hosted runners]: https://github.com/actions/virtual-environments/blob/master/images/linux/Ubuntu1804-README.md

### Summary

These are the things I would have told my one month younger self:

- Make it easy to test your code (and my younger self would have rolled his
  eyes)
- Print things that help you debug before your tests
- Make things easy for yourself with appropriate `setup` and `teardown`
  functions
- Use a bare repository in a local directory to mock a remote Git repo
- Use the existing action to set up Bats in GitHub Actions, or at least make
  sure to add Bats to your `$PATH` if you install it yourself; exit status 127
  means a dependency is missing
- Use TAP-compliant Bats output in GitHub Actions
