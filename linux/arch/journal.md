


2023-1-31
Parsec is built for X11, targeting Ubuntu 20.04 LTS.
Error "Bluetooth: hci0: Malformed MSFT vendor event: 0x02"
https://bbs.archlinux.org/viewtopic.php?id=276815

2023-1-10

emacs: error while loading shared libraries: libtiff.so.5: cannot open shared object file: No such file or directory.

libtiff.so was updated to version 6.
```shell
> ls /usr/lib/libtiff*
/usr/lib/libtiff.so  /usr/lib/libtiff.so.6  /usr/lib/libtiff.so.6.0.0  /usr/lib/libtiffxx.so  /usr/lib/libtiffxx.so.6  /usr/lib/libtiffxx.so.6.0.0
```

FIX
```
> sudo ln -s /usr/lib/libtiff.so.6.0.0 /usr/lib/libtiff.so.5
```
or recompile emacs so that emacs links with the new library.

How to manage [package] not present in AUR?

"not present in the AUR" means the package is neither in the official repo, nor in the AUR. No ris to remove those, just see what gets removed when `pacman -Rcs` them.

Display date when a package was marked out of date during update

`-Si` and `-Ss` both do this.
