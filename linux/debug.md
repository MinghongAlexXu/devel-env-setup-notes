
[How to include systemlogs in your post](https://discovery.endeavouros.com/forum-log-tool-options/how-to-include-systemlogs-in-your-post/2021/03/)


```
dmesg -l err,warn
```

Wait and print only new messages
```
dmesg -WH
```

Save
```
dmesg > dmesg.txt
```

The only difference between our two machines seems to be that you're using docker. Could you get me your installed package lists and add them to this issue. Maybe one of your packages is causing the issue.
```
dpkg -l > debs.txt
flatpak list > flatpak.txt
```

```
journalctl
```

Show which loadable kernel modules are currently loaded
```
lsmod
```

```
df -h
```
