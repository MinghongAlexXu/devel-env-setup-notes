## [Allow user-wide installations](https://wiki.archlinux.org/title/Node.js#Allow_user-wide_installations)
e.g. on fish shell:
```shell
> mkdir -p ~/.local/bin
> fish_add_path ~/.local/bin
> set -gx NPM_CONFIG_PREFIX "$HOME/.local"
```
