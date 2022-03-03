---
toc: true
...

# The perfect Bash history setup

Bash can keep a history of the commands you've issued and make them available
via multiple methods, including [Readline history commands][rlhistcmds] and
[history expansion][histexp].

An example of a Readline history command is
[`reverse-search-history`][revhist], which is usually mapped to `C-r`
(<kbd>Ctrl</kbd>+<kbd>R</kbd>), and incrementally searches the history
backward.

An example for history expansion is `!!`, an [event designator][evdesig] for
the previous command.

Having your history around is very useful, but the default settings leave a few
things on the table:

- History entries seem to disappear
- History contains duplicates and useless junk like `ls`, which you're not very
  likely ever to feel like retrieving
- Multi-line entries are folded into a single line, or split into multiple
  separate entries, or even seem to change how they're stored
- Parallel shell sessions overwrite each other's history

In this post, I explain my Bash history setup, which addresses all of these
points. "Perfect" is of course in jest---I don't think I've seen many topics
where people are as opinionated as when it comes to customizing a command-line
setup. So, "perfect for me", probably.

Note that I'm *not* going to talk about settings that change the behaviour of
history expansion, but only how history is stored.

[rlhistcmds]: <https://www.gnu.org/software/bash/manual/bash.html#Commands-For-History>
[histexp]: <https://www.gnu.org/software/bash/manual/bash.html#History-Interaction>
[revhist]: <https://www.gnu.org/software/bash/manual/bash.html#index-reverse_002dsearch_002dhistory-_0028C_002dr_0029>
[evdsig]: <https://www.gnu.org/software/bash/manual/bash.html#Event-Designators>

## Enabling history

Bash can be compiled without history support (`--enable-history` option for
`configure`), but it's enabled by default unless not supported by the operating
system.

Given history support, interactive shells have history enabled by default, but
this can also be controlled using the `history` shell option:

```bash
set -o history
```

to enable, and

```bash
set +o history
```

to disable. If enabled, the [`SHELLOPTS`][shopts] variable contains `history`,
and the status is also reported by `set -o`.

[shopts]: <https://www.gnu.org/software/bash/manual/bash.html#index-SHELLOPTS>

## Infinite history

The first nice-to-have is history that goes back all the way.

To understand how this works, we have to know a bit about how Bash stores
history. Command history is stored in multiple places: each session has an
in-memory history, which is initialized from a history file; when the session
is finished, the in-memory history is written back to the history file.

See [below][multsesh] how to configure this in a way so multiple parallel
sessions don't wipe out each other's histories; for now, we're just thinking
about a single session.

The history file by default lives at `~/.bash_history`; to put it elsewhere,
set the `HISTFILE` variable. I like my home directory uncluttered, so I have
this in my `.bashrc`:

```bash
HISTFILE="$HOME/.local/share/bash/history"
```

When the session history gets written to the history file, the file is either
overwritten, or the history is appended. This is controlled with the
`histappend` shell option:

```bash
shopt -s histappend
```

to append, and

```bash
shopt -u histappend
```

to overwrite the history file.

The size of the history is controlled with the `HISTSIZE` variable, and the
size of the history file with `HISTFILESIZE`. Common advice is to just set
these to large numbers; however, since Bash 4.3, they can be set to a negative
value for unlimited history. (Watch out, setting to `0` truncates the history
file.)

So, for an infinite history (and so far ignoring implications of parallel shell
session), we'd want something like this in our `.bashrc`:

```bash
# No clutter in the home directory
HISTFILE="$HOME/.local/share/bash/history"

# Append when storing
shopt -s histappend

# Infinite session history
HISTSIZE=-1

# Infinite history file
HISTFILESIZE=-1
```

[multsesh]: <#combine-history-from-multiple-shell-sessions>

## Control what goes into history

```bash
HISTIGNORE='exit:history:l:l[1als]:lla:+(.)'
HISTCONTROL='ignoreboth'
```

## Control how commands are stored in history

```bash
shopt -s cmdhist
shopt -s lithist
HISTTIMEFORMAT='%F %T '
```

## Combine history from different shell sessions

```bash
PROMPT_COMMAND="history -a; history -c; history -r${PROMPT_COMMAND:+$'\n'"$PROMPT_COMMAND"}"
```

## References

- Q&A with nice overview of multi-line commands:
  <https://unix.stackexchange.com/a/353407/118235>
