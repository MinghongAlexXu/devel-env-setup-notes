**TODO:**
- YASnippet snippet lib 如何创建与导入
- **[Portable]** AUCTeX [local variables](https://tex.stackexchange.com/questions/478329/auctex-fails-to-put-auxilary-files-in-a-certain-directory) 是否可以实现迁移编译设置
- [美化](https://www.reddit.com/r/emacs/comments/fzz4l6/my_fancy_splash_screen/)或实用化 splash screen
- evil-nerd-commenter 在 racket-mode 中怎么做 s-expression comment
- 挑选 Unicode font https://docs.doomemacs.org/latest/modules/ui/unicode/

## Help
`(SPC h)`

describe-function `(SPC h f)` displays the full doc of func
describe-variable `(SPC h v)`


## Installation
Emacs-git 编译安装与卸载遵循[一般模式](https://www.debian.org/doc/manuals/debian-reference/ch12.zh-cn.html#_compile_and_install_a_program)

[Optional] Backup exiting config
```shell
> mv ~/.emacs.d ~/.emacs.d.bkp
```


## Doom
Install
```shell
> git clone --depth 1 https://github.com/hlissner/doom-emacs ~/.emacs.d
> fish_add_path ~/.emacs.d/bin
> echo $fish_user_paths
> doom install
> doom doctor
```


## Vterm 
needs CMake in the PATH.


## Supported Project Types
https://docs.projectile.mx/projectile/projects.html


## See Errors from Flycheck
https://www.reddit.com/r/DoomEmacs/comments/jca63l/how_to_see_all_of_error_message/

See full errors by `SPC c x` or `:errors`

See partially by
```lisp
;; in ~/.doom.d/config.el
(after! lsp-ui
  (setq lsp-ui-sideline-diagnostic-max-lines 2))
```


## Force Load Pkgs
```lisp
(dolist (pkg '(avy expand-region helm lsp-mode))
  (require pkg))
```


## Dir Navi
https://www.reddit.com/r/emacs/comments/jws8eh/directory_navigation_in_doom_emacs/


**Tricks:**
- https://github.com/hlissner/doom-emacs/issues/2554
- https://emacs-china.org/t/weekly-tips-tricks/18171