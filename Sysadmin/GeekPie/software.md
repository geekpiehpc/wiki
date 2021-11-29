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

## Other tools

### `systemd-nspawn`

[_systemd-nspawn_](https://www.freedesktop.org/software/systemd/man/systemd-nspawn.html) is like the chroot command, but it is a _chroot on steroids_. See [systemd-nspawn - ArchWiki (archlinux.org)](https://wiki.archlinux.org/title/systemd-nspawn) and [nspawn - Debian Wiki](https://wiki.debian.org/nspawn) for introduction.

Despite using `debootstrap`, also try [`mkosi`](https://github.com/systemd/mkosi).

```bash
python3 -m pip install --user git+git://github.com/systemd/mkosi.git
sudo .local/bin/mkosi -d opensuse -t directory -p systemd-container --checksum --password password -o /var/lib/machines/opensuse-test
```

See Arch wiki for "broken colors" problem.
