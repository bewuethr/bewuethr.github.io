---
toc: true
...

# The perfect Bash history setup

Bash can keep a history of the commands you've issued and make them available
via multiple methods, including [Readline history commands] and [history
expansion].

An example of a Readline history command is [`reverse-search-history`], which
is usually mapped to `C-r` (<kbd>Ctrl</kbd>+<kbd>R</kbd>), and incrementally
searches the history backward.

An example for history expansion is `!!`, an [event designator] for the
previous command.

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

  [Readline history commands]: <https://www.gnu.org/software/bash/manual/bash.html#Commands-For-History>
  [history expansion]: <https://www.gnu.org/software/bash/manual/bash.html#History-Interaction>
  [`reverse-search-history`]: <https://www.gnu.org/software/bash/manual/bash.html#index-reverse_002dsearch_002dhistory-_0028C_002dr_0029>
  [event designator]: <https://www.gnu.org/software/bash/manual/bash.html#Event-Designators>

## Enabling history

```bash
shopt -s history
```

## Infinite history

```bash
HISTSIZE=-1
HISTFILESIZE=-1
```

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
