## themes
```shell
> mkidr /boot/EFI/refind/themes
> git clone <theme-URL> /boot/EFI/refind/themes/<theme-name>
> echo "include themes/<theme-name>/theme.conf" >> /boot/EFI/refind/refind.conf
```

## [manual boot stanzas](https://wiki.archlinux.org/title/REFInd#For_manual_boot_stanzas)

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
