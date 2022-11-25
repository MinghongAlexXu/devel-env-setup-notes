# Paru

> PKGBUILD syntax highlighting: You can install bat to enable syntax highlighting during PKGBUILD review.

[pacman usage](https://wiki.archlinux.org/title/pacman)

## Config

Configure locally:
```
$ mkdir -p ~/.config/paru/
$ cp /etc/paru.conf ~/.config/paru
```

开启颜色以及详细信息：
```
/etc/pacman.conf
----------------
# Misc options
Color
VerbosePkgLists
#ParallelDownloads = 5
```

Set file manager:
```
~/.config/paru/paru.conf
------------------------
[bin]
FileManager = ranger
```

## Problems
- [rust or rustup?](https://wiki.archlinux.org/title/rust#Installation)
- [error: no default toolchain configured](https://stackoverflow.com/questions/44303915/no-default-toolchain-configured-after-installing-rustup)
- [error while loading shared libraries: libssl.so.1.1: cannot open shared object file: No such file or directory](https://www.reddit.com/r/archlinux/comments/yn5o8a/comment/iv7qkme/)


# PKGBUILD