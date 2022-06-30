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

## [Persistent Block Device Naming](https://wiki.archlinux.org/title/Persistent_block_device_naming)

GPT 中的 *PARTLABEL* 和 *PARTUUID* 只能用 `blkid` 查看。 其他 naming methods 也可用 `lsblk -f` 查看。

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

## 挑支持 wayland 的工具

https://github.com/natpen/awesome-wayland

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
