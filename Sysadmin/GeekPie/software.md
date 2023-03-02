---
title: software
description: 
published: true
date: 2022-03-31T03:46:11.791Z
tags: 
editor: markdown
dateCreated: 2022-03-21T02:22:18.396Z
---

# Software

Currently we are using [Ubuntu Server 20.04.3 LTS](https://releases.ubuntu.com/20.04/).

## Boot: `systemd-boot`

We have replaced `grub` with `systemd-boot`. For introduction, see [systemd-boot - ArchWiki (archlinux.org)](https://wiki.archlinux.org/title/systemd-boot).

To configure `systemd-boot`, use `bootctl`.
To change kernel parameters, modify `/etc/kernel/postinst.d/zz-update-systemd-boot`.
GitHub backup: https://gist.github.com/KiruyaMomochi/9df313c2abc55c1736d457d48abc0f54

## Network: `netplan`

> Since Systemd v197, network interfaces use predictable naming schemes. See [systemd.net-naming-scheme (www.freedesktop.org)](https://www.freedesktop.org/software/systemd/man/systemd.net-naming-scheme.html) for detail.

Ubuntu use [netplan](https://netplan.io/) to configure network. It reads network configuration from `/etc/netplan/*.yaml`, then convert them to [systemd-networkd](https://www.freedesktop.org/software/systemd/man/systemd.network.html) configuration.

Netplan configuration examples: https://netplan.io/examples/.

## Drivers

### InfiniBand

1. Download drivers from [Linux InfiniBand Drivers (mellanox.com)](https://www.mellanox.com/products/infiniband-drivers/linux/mlnx_ofed).
2. `tar -xzf MLNX_OFED_LINUX-5.4-3.1.0.0-ubuntu20.04-x86_64.tgz`
3. `cd` to the directory and
```bash
sudo ./mlnxofedinstall --add-kernel-support
```

#### Configure IPoIB

For RHEL/CentOS, see [IP over InfiniBand (IPoIB) - MLNX_OFED v5.1-0.6.6.0 - Mellanox Docs](https://docs.mellanox.com/pages/viewpage.action?pageId=32413276).

For Ubuntu, create `/etc/netplan/10-infiniband.yaml` with:

```yaml
network:
    version: 2
    ethernets:
        ibp161s0f0: # Name of the InfiniBand interface
            addresses:
                - 11.4.3.20/24 # Change to your IP address
```

You may need to change interface name and ip address to your own.

## Ansible

To manage two server at the same time, it's easier to use [Ansible](https://www.ansible.com/).

## Network File System (NFS)

NFS is exported from `node1`. Only NFS v4 is supported:
```
/srv/nfs4 *(rw,sync,fsid=0,crossmnt,no_subtree_check)
/srv/nfs4/home *(rw,sync,no_subtree_check)
```

/proc/fs/nfsd/versions: `-2 -3 +4 +4.1 +4.2`

It is mounted on all nodes at `/mnt/nfs4` and `/mnt/home`:
```
node1:/ /mnt/nfs4 nfs rw,noauto,x-systemd.automount 0 0
node1:/home /mnt/home nfs rw 0 0
```

You can use `/mnt/home/<user>` as your home directory:
```bash
# On node 1
sudo mkdir /srv/nfs4/home/<user>
# On all nodes
sudo usermod -d /mnt/home/<user> <user>
```

## Other tools

### `systemd-nspawn`

See [systemd-nspawn](./systemd-nspawn.md)
