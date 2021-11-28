# Software

Currently we are using [Ubuntu Server 20.04.3 LTS](https://releases.ubuntu.com/20.04/).

## Boot: `systemd-boot`

We have replaced `grub` with `systemd-boot`. For introduction, see [systemd-boot - ArchWiki (archlinux.org)](https://wiki.archlinux.org/title/systemd-boot).

To configure `systemd-boot`, use `bootctl`.
To change kernel parameters, modify `/etc/kernel/postinst.d/zz-update-systemd-boot`.

## Network: `netplan`

Ubuntu use [netplan](https://netplan.io/) to configure network. It reads network configuration from `/etc/netplan/*.yaml`, then convert them to [systemd-networkd](https://www.freedesktop.org/software/systemd/man/systemd.network.html) configuration. 

Netplan configuration examples: https://netplan.io/examples/.

## Ansible

To manage two server at the same time, it's easier to use [Ansible](https://www.ansible.com/).
