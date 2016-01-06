---
published: false
---



## Solarize everything

* Console ([GNOME terminal](https://github.com/Anthony25/gnome-terminal-colors-solarized), [mintty](https://github.com/karlin/mintty-colors-solarized))
* [Dircolors](https://github.com/seebi/dircolors-solarized)
* [Vim](https://github.com/altercation/vim-colors-solarized)
* Grep `mt=01;38;5;166:sl=:cx=00;38;5;240:fn=00;38;5;245:ln=00;38;5;64:bn=00;38;5;125:se=00;38;5;136`
* [Tmux](https://github.com/seebi/tmux-colors-solarized)
* Prompt?

```
for i in {0..255}; do
    echo -en "\e[07;38;5;${i}m  \e[0m"
    if (( (i+3) % 6 == 0 )); then echo; fi
done
```