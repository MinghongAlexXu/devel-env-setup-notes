Connect to a network [using iwd](https://wiki.archlinux.org/title/Iwd#Connect_to_a_network)

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