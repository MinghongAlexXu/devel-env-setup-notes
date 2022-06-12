## Running Linux GUI apps (X11 and Wayland)

**Option 1**, X server on the host:
1. Install a Windows X server, VcXsrv.
2. Start VcXsrv while access control is disabled.
3. Tell the WSL where the X server is: `export DISPLAY=$(ip route|awk '/^default/{print $3}'):0.0`.

**Option 2**, [WSLg](https://github.com/microsoft/wslg)
