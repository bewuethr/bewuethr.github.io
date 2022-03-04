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
