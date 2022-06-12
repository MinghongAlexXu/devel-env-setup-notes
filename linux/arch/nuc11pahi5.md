# intel NUC11PAH

## Hardware Compabilities

### Supported operating systems
https://www.intel.com/content/www/us/en/support/articles/000005628/intel-nuc.html

### UEFI, drivers, and firmwares

https://www.intel.com/content/www/us/en/products/sku/205040/intel-nuc-11-performance-kit-nuc11pahi5/downloads.html

UEFI update instructions: https://www.intel.com/content/www/us/en/support/articles/000033291/intel-nuc.html

### Screen Flickering

BIOS PATGL357 版本，0035，太老导致。更新为 [0042](https://www.intel.com/content/www/us/en/download/19694/bios-update-patgl357.html
) 后解决。

Ref: [英特尔® NUC 迷你电脑上闪烁的显示器故障排除](https://www.intel.cn/content/www/cn/zh/support/articles/000031497/intel-nuc.html
)

## Installation

## paru

> PKGBUILD syntax highlighting: You can install bat to enable syntax highlighting during PKGBUILD review.

[pacman usage](https://wiki.archlinux.org/title/pacman)

开启颜色以及详细信息
```
/etc/pacman.conf
----------------
# Misc options
Color
VerbosePkgLists
#ParallelDownloads = 5
```

```
$ mkdir -p ~/.config/paru/
$ cp /etc/paru.conf ~/.config/paru
$ vim ~/.config/paru/paru.conf
```
```
~/.config/paru/paru.conf
------------------------
[bin]
FileManager = ranger
```

Problems:
- [rust or rustup?](https://wiki.archlinux.org/title/rust#Installation)
- [error: no default toolchain configured](https://stackoverflow.com/questions/44303915/no-default-toolchain-configured-after-installing-rustup)

## xx instead of xx

新建一个专门存放 aliases 的文件 `~/.bash_aliases`，并在 `~/.bashrc` 中 include 它
```
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
```

### [exa](https://the.exa.website/) instead of ls

`~/.bash_aliases`
```
alias ls='exa'
alias la='exa -a'
alias ll='exa -alh'
```

### [bat](https://github.com/sharkdp/bat) instead of cat

bat 中自带 less

### [ripgrep](https://github.com/BurntSushi/ripgrep) (rg) instead of grep

> When searching for files (as opposed to matching lines in files) you might try `fd` instead of ripgrep. You can pass an `--extension` flag though to `fd` to filter by filetype. `fd` uses some of the same libraries as ripgrep, but is optimized for listing files while ripgrep is optimized for searching within files.

### [fd](https://github.com/sharkdp/fd) instead of find

### tldr instead of man

### [HTTPie](https://github.com/httpie/httpie) instead of curl and wget

## [Persistent Block Device Naming](https://wiki.archlinux.org/title/Persistent_block_device_naming)

GPT 中的 *PARTLABEL* 和 *PARTUUID* 只能用 `blkid` 查看。 其他 naming methods 也可用 `lsblk -f` 查看。

## rEFInd

### themes
```
[/]$ mkidr /boot/EFI/refind/themes
[/]$ git clone <theme-URL> /boot/EFI/refind/themes/<theme-name>
[/]$ echo "include themes/<theme-name>/theme.conf" >> /boot/EFI/refind/refind.conf
```

### [manual boot stanzas](https://wiki.archlinux.org/title/REFInd#For_manual_boot_stanzas)

/boot/EFI/refind/refind.conf
```
[...]
menuentry "Arch Linux" {
	icon     /EFI/refind/themes/<theme-dir>/icons/os_arch.png
	#volume   "Arch Linux"
	loader   /vmlinuz-linux
	initrd   /initramfs-linux.img
	options  "root=PARTUUID=XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX rw rootfstype=btrfs initrd=intel-ucode.img intel_pstate=no_hwp snd_hda_codec_hdmi.enable_silent_stream=0"
	submenuentry "Boot using fallback initramfs" {
		initrd /initramfs-linux-fallback.img
	}
	submenuentry "Boot to terminal" {
		add_options "systemd.unit=multi-user.target initrd=intel-ucode.img"
	}
    #disabled
}
[...]
include themes/<theme-dir>/theme.conf
```
> **Kernel parameters** are set with the options keyword.
> 
> If you need additional initrds (e.g. for **Microcode**), you can specify them in options.

*PARTUUID* 设置为 `/boot` 的。这样对于 rEFInd 来说根分区就是 `/boot`；所以 `/vmlinuz-linux` 就是 `/boot/vmlinuz-linux`。同理 `/EFI`、`/initramfs-linux.img`、`/initramfs-linux-fallback.img`、 和 `/intel-ucode.img`。

`snd_hda_codec_hdmi.enable_silent_stream=0` 含义不明。但是可以用来消除 *kernel ring buffer* 中的报错 `snd_hda_codec_hdmi hdaudioc0d2: monitor plugged-in, failed to power up codec ret=[-13]`。

`rootfstype=btrfs` 和 `intel_pstate=no_hwp` 作用不明。

### 清理启动选项

```
dont_scan_dirs /EFI/BOOT,/EFI/systemd
dont_scan_files vmlinuz-linux
```

> You can rid yourself of the unwanted boot menu by deleting the old files or by using `dont_scan_dirs` or `dont_scan_files` in `refind.conf`. Before you do this, you should use rEFInd to identify the unwanted files—the filename and volume identifier appear under the icons when you highlight the option. Before you delete the old files, though, you may want to copy over any changes you've made to the rEFInd configuration, icons, and other support files.
> 
> https://www.rodsbooks.com/refind/installing.html

Alternatively,

> In rEFInd bootloader menu, you can hide the excess entries by selecting the extra entries by arrow keys in keyboard and then pressing the Delete key! A confirmation will pop up...upon selecting yes, the entry gets hidden and your bootloader is good to go!!
> 
> https://askubuntu.com/questions/630258/refind-question-remove-multiple-boot-items

### [其他配置](https://arch.icekylin.online/advanced/optional-cfg-2.html#🔍-refind)

```
timeout 20
resolution 3840 2160
extra_kernel_version_strings linux-hardened,linux-zen,linux-lts,linux
```

## Fontconfig

太长。见另一文件。

## 挑支持 wayland 的工具

https://github.com/natpen/awesome-wayland

## Sway

Copy the sample config to the home and begin editting
```
$ mkdir ~/.config/sway/
$ cp /etc/sway/config ~/.config/sway/
$ vim ~/.config/sway/config
```
```
# Default config for sway
#
# Copy this to ~/.config/sway/config and edit it to your liking.
#
# Read `man 5 sway` for a complete reference.

# gaps
gaps inner 5
gaps outer 1
# gaps used if only more than one container on the workspace
# disable window titlebar
default_border none
[...]
```
vim `:w` 之后 <kbd>Mod</kbd>+<kbd>Shift</kbd>+<kbd>C</kbd> 重载配置观察效果。

Get the list of current outputs
```
$ sway -t get_outputs
```

使用 waybar 替换默认的 status bar, swaybar
```
bar {
    swaybar_command waybar
}
```

使用 foot 替换默认的 alacritty
```
set $term foot
```

打字时隐藏光标
```
seat * hide_cursor when-typing enable
```

```
swaymsg -t get_outputs
```

## waybar

```
$ mkdir ~/.config/waybar
$ cp /etc/xdg/waybar/* ~/.config/waybar
```

`~/.config/waybar/style.css`
```
* {
    /* `otf-font-awesome` is required to be installed for icons */
    font-family: Roboto, Helvetica, Arial, sans-serif;
    font-size: 13px;    
}
```
```
$ paru -S otf-font-awesome
```

换成 Iosevka Etoile 字体
```
$ paru -S ttc-iosevka-etoile
$ fc-list : family | rg "Iosevka Etoile"
```
`~/.config/waybar/style.css`
```
* {
    /* `otf-font-awesome` is required to be installed for icons */
    font-family: "Iosevka Etoile Light Oblique", sans-serif;
    font-size: 13px;    
}
```

https://github.com/Alexays/Waybar/wiki/Module:-Custom


### Info

terminal 中运行 waybar 可以查看 log

> The sway/language module displays the current keyboard layout in Sway.
>
> https://github.com/Alexays/Waybar/wiki/Module:-Language

### Problems

没安装 mpd。不知道 mpd 的用处。

[warning] module keyboard-state: Disabling module "keyboard-state", Failed to find keyboard device

module "temperature" 温度数值不正常

[不支持 pipwire](https://github.com/Alexays/Waybar/issues/933)

[Emojis are too big](https://github.com/Alexays/Waybar/issues/1233)

## bemenu (deprecated)

> https://github.com/swaywm/sway/issues/5112#issuecomment-834209083
> 
> emersion commented on May 7, 2021
>
> I don't personally like bemenu, because its codebase is kind of a mess and it has dynamically loadable backends which make everything more complicated.
>
> ---
>
> primeos commented on May 15, 2021
>
> @emersion ok, that makes sense, thanks for the reply. I assume you don't like the other ones either (AFAIK you use https://git.sr.ht/~emersion/dotfiles/tree/master/item/bin/menu)? I briefly looked at the rest of the alternatives from the two lists above and it looks like they all have some issues (look too different, too complex, too many dependencies, etc. - https://github.com/nyyManni/dmenu-wayland might have potential but I guess a dependency on glib-2.0 and gobject-2.0 is also not ideal(?)).
>
> >Maybe it's time to reconsider whether to use dmenu in the default configuration?
>
> So I guess it's best to stick with dmenu (at least for now)?

## fzf-based app launcher

```
~/.config/sway/config
---------------------
# Logo key. Use Mod1 for Alt.
set $mod Mod4
[...]
# Minimal fzf application launcher
for_window [app_id="^launcher$"] floating enable, sticky enable, border pixel 10, resize set width 16 ppt height 25 ppt
set $menu foot --app-id 'launcher' bash -c 'compgen -c | sort -u | fzf --layout=reverse --bind=enter:replace-query+print-query | xargs --no-run-if-empty swaymsg exec --'
#set $menu alacritty --class 'launcher' --command  bash -c 'compgen -c | sort -u | fzf --bind=enter:replace-query+print-query | xargs --no-run-if-empty swaymsg exec --'
bindsym $mod+space exec $menu
[...]
# Start your launcher
#bindsym $mod+d exec $menu
[...]
# Swap focus between the tiling area and the floating area
bindsym $mod+d focus mode_toggle
```

Problems:
- [Determine whether a program is a GUI or console application in Linux](https://stackoverflow.com/questions/16550112/determine-whether-a-program-is-a-gui-or-console-application-in-linux)
- [Print Query if there is no match](https://github.com/junegunn/fzf/issues/1693#issuecomment-699642792)
- TODO: Switch focus to launcher if it's running or start it if it's not (Scanning `swaymsg -t get_tree` to find app_id)

## [foot](https://codeberg.org/dnkl/foot) | a fast, lightweight and minimalistic Wayland terminal emulator

## Kmonad

```lisp
~/.config/kmonad/config.kbd
---------------------------
;; Layout Template with a Column Width of 8 Characters
;; (defsrc
;;   _      _      _      _      _      _      _      _      _      _      _      _      _      _      _      _
;;   _      _      _      _      _      _      _      _      _      _      _      _      _      _             _
;;   _      _      _      _      _      _      _      _      _      _      _      _      _      _             _
;;   _      _      _      _      _      _      _      _      _      _      _      _      _                    _
;;   _      _      _      _      _      _      _      _      _      _      _      _                    _      _
;;   _      _      _                    _                    _      _      _                    _      _      _
;; )

(defcfg
  ;; For Linux
  input  (device-file "/dev/input/by-id/usb-Keychron_Keychron_K2-event-kbd")
  output (uinput-sink "Keychron K2")

  fallthrough true
  allow-cmd   false
)

(defsrc
  esc    f1     f2     f3     f4     f5     f6     f7     f8     f9     f10    f11    f12    ins    prnt   del
  grv    1      2      3      4      5      6      7      8      9      0      -      =      bspc          pgup
  tab    q      w      e      r      t      y      u      i      o      p      [      ]      \             pgdn
  caps   a      s      d      f      g      h      j      k      l      ;      '      ret                  home
  lsft   z      x      c      v      b      n      m      ,      .      /      rsft                 up     end
  lctl   lalt   lmet                 spc                  rmet   cmp    rctl                 left   down   rght
)

(defalias 
  fn    (around (layer-toggle function) cmp)
  cmkdh (layer-switch colemak-mod-dh)
  us    (layer-switch united-states)
  esctl (tap-next esc lctl)
)

;; Default Layout
(deflayer colemak-mod-dh
  esc    f1     f2     f3     f4     f5     f6     f7     f8     f9     f10    f11    f12    @us    prnt   del
  grv    1      2      3      4      5      6      7      8      9      0      -      =      bspc          pgup
  tab    q      w      f      p      b      j      l      u      y      ;      [      ]      \             pgdn
  @esctl a      r      s      t      g      m      n      e      i      o      '      ret                  home
  lsft   x      c      d      v      z      k      h      ,      .      /      rsft                 up     end
  lctl   lalt   lmet                 spc                  rmet   @fn    rctl                 left   down   rght
)

(deflayer function
  _      brdn   brup   _      _      _      _      _      _      _      mute   vold   volu   _      _      _
  _      _      _      _      _      _      _      _      _      _      _      _      _      _             _
  _      _      _      _      _      _      _      _      _      _      _      _      _      _             _
  _      _      _      _      _      _      _      _      _      _      _      _      _                    _
  _      _      _      _      _      _      _      _      _      _      _      _                    _      _
  _      _      _                    _                    _      _      _                    _      _      _
)

(deflayer united-states
  esc    f1     f2     f3     f4     f5     f6     f7     f8     f9     f10    f11    f12    @cmkdh prnt   del
  grv    1      2      3      4      5      6      7      8      9      0      -      =      bspc          pgup
  tab    q      w      e      r      t      y      u      i      o      p      [      ]      \             pgdn
  @esctl a      s      d      f      g      h      j      k      l      ;      '      ret                  home
  lsft   z      x      c      v      b      n      m      ,      .      /      rsft                 up     end
  lctl   lalt   lmet                 spc                  rmet   @fn    rctl                 left   down   rght
)
```

```
/etc/systemd/system/kmonad.service
--------------
[Unit]
Description=kmonad keyboard config

[Service]
Restart=always
RestartSec=3
ExecStartPre=/usr/bin/sleep 1
ExecStart=/usr/bin/kmonad /home/devel/.config/kmonad/config.kbd
# %E, configuration directory root, is either /etc/ (for the system manager) or the path "$XDG_CONFIG_HOME" resolves to (for user managers).
# https://www.freedesktop.org/software/systemd/man/systemd.unit.html
#ExecStart=/usr/bin/kmonad %E/kmonad/config.kbd
Nice=-20

[Install]
WantedBy=default.target
```

## qutebrowser

This is a QT application.
> To enable Wayland support in Qt 5 or 6, install the qt5-wayland or qt6-wayland package, respectively.
>
> https://wiki.archlinux.org/title/Wayland#Qt

issue: [Flashing white screen](https://github.com/qutebrowser/qutebrowser/issues/6939)


## HDMI audio

> If you alternatively succeed with
>
> ```
> $ speaker-test -Dplug:hdmi
> ```
> for your HDMI or DisplayPort port the following configuration will work:
> ```
> ~/.asoundrc
> -------------
> pcm.!default {
>     type plug
>     slave.pcm "hdmi"
> }
> ```
>
> https://wiki.archlinux.org/title/Advanced_Linux_Sound_Architecture/Troubleshooting#HDMI_Output_does_not_work

## Podman

## Screen Flickering

```shell
> sudo dmseg -l err,warn
[...]
i915 0000:00:02.0: [drm] *ERROR* CPU pipe A FIFO underrun: transcoder,
```
[i5-10210U Screen Flickering [drm] *ERROR* CPU pipe A FIFO underrun](https://bbs.archlinux.org/viewtopic.php?id=263720)
> temporary fix is to put
> ```
> intel_idle.max_cstate=4
> ```
> in the kernel commandline.

[What are CPU "C-states" and how to disable them if needed?](https://gist.github.com/Brainiarc7/8dfd6bb189b8e6769bb5817421aec6d1)
https://wiki.archlinux.org/title/intel_graphics

## Enable Early KMS
https://wiki.archlinux.org/title/Intel_graphics#Enable_early_KMS
https://wiki.archlinux.org/title/Kernel_mode_setting#Early_KMS_start

## Clipboard

wl-clipboard

## Neovim

Clipboard integration (:help clipboard): Install wl-clipboard

Python3 plugin support (:help python): Install python-neovim

## libicuuc.so.70

Pro:
```
error while loading shared libraries: libicuuc.so.70: cannot open shared object file: No such file or directory
```

Sol:
> Icu update will always break every AUR package depending on it, so you need to recompile those packages. Also, make sure that your system and mirrorlist are up-to-date.

Search:
```
paru -Qs icu
cd / && fd libicuuc.so
```
