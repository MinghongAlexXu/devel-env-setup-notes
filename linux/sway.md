## Config

Copy the sample config to the home and begin editting:
```
$ mkdir ~/.config/sway/
$ cp /etc/sway/config ~/.config/sway/
```

Gaps between windows:
```
gaps inner 5
gaps outer 1
```

Disable window titlebar:
```
default_border none
```

使用 waybar 替换默认的 status bar, swaybar:
```
bar {
    swaybar_command waybar
}
```

Terminal emulator 用 foot:
```
set $term foot
```

App launcher 用 fuzzel:
```
set $menu fuzzel --terminal foot --font=monospace:size=17 --lines 15 --width 40 --prompt=">> "
bindsym $mod+space exec $menu
```

打字时隐藏光标:
```
seat * hide_cursor when-typing enable
```

## Tips

- Read `man 5 sway` for a complete reference of config.
- <kbd>Mod</kbd>+<kbd>Shift</kbd>+<kbd>C</kbd> 重载配置观察效果。
- https://github.com/swaywm/sway/tree/master/completions/fish
  - `swaymsg -t get_outputs` prints the list of current outputs.
- https://www.reddit.com/r/swaywm/comments/mtguzy/comment/guzk9gk/

## 修复记录
- [2022/06/30](https://www.reddit.com/r/Gentoo/comments/s1gt8b/comment/hsazbkg): Install [polkit](https://wiki.archlinux.org/title/Polkit)