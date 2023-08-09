---
summary: >-
  The second article in the Bash history setup series: what goes into the
  history?
---

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
time without accessing history, and b) only useful in the context or working
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
interested in, but I'd rather have that than missing out on stuff I *do* want
to see later on.

[`history`]: <https://www.gnu.org/software/bash/manual/bash.html#index-history>
[`fc`]: <https://www.gnu.org/software/bash/manual/bash.html#index-fc>
[cdalias]: <https://github.com/bewuethr/dotfiles/blob/7c5e5438a6641855cd473a3e58524e384e24a58d/.aliases.sh#L1-L2>
[`HISTIGNORE`]: <https://www.gnu.org/software/bash/manual/bash.html#index-HISTIGNORE>
[lsaliases]: <https://github.com/bewuethr/dotfiles/blob/7c5e5438a6641855cd473a3e58524e384e24a58d/.aliases.sh#L4-L16>

## General history control settings

For more general control of what goes into history, Bash provides a variable
[`HISTCONTROL`]. It is also colon-separated, with values taken from this list:

- `ignorespace`: ignore commands starting with a space character

  ```bash
  $ echo "Hello"
  Hello
  $  echo "Starts with blank"
  Starts with blank
  $ history | tail -n2
      6  echo "Hello"
      7  history | tail -n2
  ```

- `ignoredups`: don't store commands that exactly match the last stored command

  ```bash
  $ echo 1
  1
  $ echo 2
  2
  $ echo 2
  2
  $ echo 3
  3
  $ history | tail -n4
      9  echo 1
     10  echo 2
     11  echo 3
     12  history | tail -n4
  ```

- `ignoreboth` is the same as `ignorespace:ignoredups`

  ```bash
  $ echo 1
  1
  $  echo "Start with blank"
  Start with blank
  $ echo 2
  2
  $ echo 2
  2
  $ history | tail -n3
     18  echo 1
     19  echo 2
     20  history | tail -n3
  ```

- `erasedups` removes all lines from history matching the current command
  before storing it

  ```bash
  $ echo 1
  1
  $ echo 2
  2
  $ history | tail -n3
     16  echo 1
     17  echo 2
     18  history | tail -n3
  $ echo 3
  3
  $ echo 2
  2
  $ history | tail -n5
     15  echo 1
     16  history | tail -n3
     17  echo 3
     18  echo 2
     19  history | tail -n5
  ```

I use `HISTCONTROL='ignoreboth'`; the only reason I'm not using
`'ignoreboth:erasedups'` is because sometimes, I want to repeat a sequence of
commands from history using <kbd>Ctrl</kbd>+<kbd>O</kbd> (execute the current
line from history and fetch the next one, [`operate-and-get-next`]), and
`erasedups` might remove commands from the middle of such sequences.

Realistically, though, that's a pretty unlikely edge case, and I might enable
`erasedups` at some point. It would get rid of more than 7,500 commands in my
history, apparently!

[`HISTCONTROL`]: <https://www.gnu.org/software/bash/manual/bash.html#index-HISTCONTROL>
[`operate-and-get-next`]: <https://www.gnu.org/software/bash/manual/bash.html#index-operate_002dand_002dget_002dnext-_0028C_002do_0029>

## The setup so far

Taken all together, our history setup now provides infinite history, and only
stores what we want:

```bash
# No clutter in the home directory
HISTFILE="$HOME/.local/share/bash/history"

# Overwrite when storing
shopt -u histappend

# Infinite session history
HISTSIZE=-1

# Infinite history file
HISTFILESIZE=-1

# Don't store commands that aren't useful in history
HISTIGNORE='exit:history:l:l[1als]:lla:+(.)'

# Ignore commands starting with space, and duplicates
HISTCONTROL='ignoreboth'
```

In the next post, we'll examine how to get multi-line commands into history,
and add timestamps to commands.
