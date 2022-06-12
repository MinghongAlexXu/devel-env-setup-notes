# Fish

## Set to defualt

1. `brew install fish`
2. Check the path with `which fish` which returns `/opt/homebrew/bin/fish`.
3. Add fish to the know shells by `sudo sh -c 'echo /opt/homebrew/bin/fish >> /etc/shells'`.
4. Restart the terminal.
5. Set fish as the default shell: `chsh -s /opt/homebrew/bin/fish`.
6. Restart the terminal and check if it launched with fish or not.

## Path

Add path using [fish_add_path](https://fishshell.com/docs/current/cmds/fish_add_path.html).

Check fish paths by `echo $fish_user_paths`.

## Misc

- [Define aliases](https://stackoverflow.com/questions/2762994/define-an-alias-in-fish-shell)
- [Interactive use](https://fishshell.com/docs/current/interactive.html)
- [Greeting message](https://stackoverflow.com/a/19291140/18955467)



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
