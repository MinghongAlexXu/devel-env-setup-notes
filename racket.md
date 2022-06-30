## Install toolchain (racket, raco, and drracket)

直接安装包含 drracket 的 racket，省力。或者安装 racket-minimal 后 `raco pkg install --auto drracket` 并添加 drracket 到搜索路径。

## Doom Emacs

```lisp
~/.doom.d/init.el
-----------------
(racket +xp)  ;; lsp 还不完整
```

在 ~/.doom.d/init.el 开启 racket，doctor sync 后会自动安装 racket-mode。Note: racket-mode needs drracket Racket package.

`M-x` `racket-mode-start-faster`: Compile Racket Mode’s .rkt files for [faster startup](https://www.racket-mode.com/#racket_002dmode_002dstart_002dfaster).
