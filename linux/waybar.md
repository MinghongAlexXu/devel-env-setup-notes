## Config

Config template
```
$ mkdir ~/.config/waybar
$ cp /etc/xdg/waybar/* ~/.config/waybar
```

换成 Iosevka Etoile 字体:
```shell
> paru -S ttc-iosevka-etoile
> fc-list : family | rg "Iosevka Etoile"
```
```
~/.config/waybar/style.css
--------------------------
* {
    /* `otf-font-awesome` is required to be installed for icons */
    font-family: "Iosevka Etoile Light Oblique", sans-serif;
    font-size: 13px;    
}
```

https://github.com/Alexays/Waybar/wiki/Module:-Custom


## Tips

terminal 中运行 waybar 可以查看 log

## Problems

没安装 mpd。不知道 mpd 的用处。

[warning] module keyboard-state: Disabling module "keyboard-state", Failed to find keyboard device

module "temperature" 温度数值不正常

[不支持 pipwire](https://github.com/Alexays/Waybar/issues/933)

[Emojis are too big](https://github.com/Alexays/Waybar/issues/1233)
