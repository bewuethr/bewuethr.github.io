# The perfect Bash history setup: not retaining garbage

In the [first post], we looked at configuring an interactive shell session such
that it retains infinite command history. The recommended setup looks like
this:

```sh
HISTFILE="$HOME/.local/share/bash/history"
shopt -u histappend
HISTSIZE=-1
HISTFILESIZE=-1
```

See the post for all the details.

[first post]: <2022-03-03-infinite-history.html>

## Ignoring specific commands

If we think of the command history as a list of commands that might be handy
for reuse at some point, we probably don't want absolutely everything in there.
For example, `ls` and related commands are a) short and easy to write at any
time without accessing history and b) only useful in the context or working
directory they were originally issued in, which we don't really have available.

Another class of commands I never recall from history are those interacting
with the history itself, such as [`history`] and [`fc`].

In my dotfiles, I have [aliases for `cd ..` and `cd ../..`][cdalias]; I don't
need those in history either.

And finally, I'm not interested in `exit`.

To specify which commands shouldn't go into history, Bash provides the
[`HISTIGNORE`] variable. It's a colon-separated list of patterns, and commands
matching the pattern (anchored at the beginning of the line) aren't stored to
history.

For example, for the commands described above as not interesting to me, I can
set

```bash
HISTIGNORE='exit:history:l:l[1als]:lla:+(.)'
```

to ignore `exit`, `history`, most of [my `ls` aliases][lsaliases][^1], and
commands consisting of any number of periods. `+(.)` is an extended glob
pattern meaning "one or more periods"; `HISTIGNORE` understands extended globs
if the `extglob` shell option is enabled, which by default it is in interactive
shells.

[^1]: I realize looking at them right now that I should probably update my
`HISTIGNORE`!

The list is probably rather conservative and stores some commands I'm not
interested in, but I'd rather have that than ignoring stuff I *do* want to see
later on.

[`history`]: <https://www.gnu.org/software/bash/manual/bash.html#index-history>
[`fc`]: <https://www.gnu.org/software/bash/manual/bash.html#index-fc>
[cdalias]: <https://github.com/bewuethr/dotfiles/blob/7c5e5438a6641855cd473a3e58524e384e24a58d/.aliases.sh#L1-L2>
[`HISTIGNORE`]: <https://www.gnu.org/software/bash/manual/bash.html#index-HISTIGNORE>
[lsaliases]: <https://github.com/bewuethr/dotfiles/blob/7c5e5438a6641855cd473a3e58524e384e24a58d/.aliases.sh#L4-L16>

## General history control settings

```bash
HISTIGNORE='exit:history:l:l[1als]:lla:+(.)'
HISTCONTROL='ignoreboth'
```
