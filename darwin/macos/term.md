# Fish

## Set to defualt

1. `brew install fish`
2. Check the path with `which fish` which returns `/opt/homebrew/bin/fish`.
3. Add fish to the know shells by `sudo sh -c 'echo /opt/homebrew/bin/fish >> /etc/shells'`.
4. Restart the terminal.
5. Set fish as the default shell: `chsh -s /opt/homebrew/bin/fish`.
6. Restart the terminal and check if it launched with fish or not.

## Tips and tricks

### Autosuggestions
- `Control+F` accept the autosuggestion
- `Alt+F` accept the first suggested word

### Path
- Add path using [fish_add_path](https://fishshell.com/docs/current/cmds/fish_add_path.html).
- Check fish paths by `echo $fish_user_paths`.

### [Command line editor](https://fishshell.com/docs/current/interactive.html#command-line-editor)

- `Alt+E` edit the current command line in an external editor. 
- The editor is chosen from the first available of the `$VISUAL` or `$EDITOR` variables.
- `Alt+V` same as `Alt+E`.

## Misc

- [define aliases](https://stackoverflow.com/questions/2762994/define-an-alias-in-fish-shell)
- [environment variable](https://fishshell.com/docs/current/faq.html#how-do-i-set-or-clear-an-environment-variable)
- [interactive use](https://fishshell.com/docs/current/interactive.html)
- [greeting message](https://stackoverflow.com/a/19291140/18955467)
- [incremental history search](https://superuser.com/questions/627929/is-there-a-reverse-incremental-search-functionality-in-fish-similar-to-bash-s) 



# Xonsh

`brew install xonsh`

## Tab completion

- To use bash completion files, `brew install bash-completion@2`.
- To use fish completion, `xontrib load fish_completer`.

## RC files

1. `mkdir ~/.config/xonsh/rc.d`
2. Create `options.xsh` to manage [customisation options](https://xon.sh/envvars.html).
3. Create `aliases.xsh` to manage aliases.
4. Create `paths.xsh` to manage paths.

[Doc](https://xon.sh/xonshrc.html) and [interesting RC files](https://github.com/anki-code/awesome-xonshrc).

## Extensions (Xontribs)

- offical [tutorial](https://xon.sh/tutorial_xontrib.html)
- [list](https://github.com/xonsh/awesome-xontribs) of extensions

## Misc

- [cheatsheet](https://github.com/anki-code/xonsh-cheatsheet/blob/main/README.md)



# Terminal Emulators

## kitty

Download the [sample kitty.conf](https://sw.kovidgoyal.net/kitty/conf/#sample-kitty-conf) and put it into `~/.config/kitty`.

## iTerm2
