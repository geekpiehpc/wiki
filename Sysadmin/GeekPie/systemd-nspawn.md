# `systemd-nspawn`

[_systemd-nspawn_](https://www.freedesktop.org/software/systemd/man/systemd-nspawn.html) is like the chroot command, but it is a _chroot on steroids_. See [systemd-nspawn - ArchWiki (archlinux.org)](https://wiki.archlinux.org/title/systemd-nspawn) and [nspawn - Debian Wiki](https://wiki.debian.org/nspawn) for introduction.

## Bootstrap

We can bootstrap a Debian machine using `debootstrap`, but also try [`mkosi`](https://github.com/systemd/mkosi).

For example, bootstrap a openSUSE image:

```bash
python3 -m pip install --user git+git://github.com/systemd/mkosi.git
sudo .local/bin/mkosi -d opensuse -t directory -p systemd-container --checksum --password password -o /var/lib/machines/opensuse-test
```

## RDMA

### Install

Although there is no document for systemd-nspawn, we can refer to [How-to: Deploy RDMA accelerated Docker container over InfiniBand fabric](https://docs.mellanox.com/pages/releaseview.action?pageId=15049785).

> Make sure these tools has the same version as host.

We only need to install userspace tools into nspawn container without updating firmware:

```bash
./mlnxofedinstall --user-space-only --without-fw-update
```

### Edit `.nspawn` file

Edit `.nspawn` file of the container, which is located at `/etc/systemd/nspawn/<machine-name>.nspawn`.
If such a file does not exist, create one.

Then, add following content

```conf
[Exec]
Capability=CAP_IPC_LOCK
LimitMEMLOCK=infinity

[Files]
Bind=/dev/infiniband/
Bind=/dev/hugepages
```

Also consider use host network by

```conf
[Network]
VirtualEthernet=no
```

### Add `DeviceAllow`

Create a drop-in file use command

```bash
sudo systemctl edit systemd-nspawn@<machine-name>
```

with content of

```conf
[Service]
DeviceAllow=/dev/infiniband/uverbs0 rwm
DeviceAllow=/dev/infiniband/uverbs1 rwm
```

Put all of devices you want to allow there.

### Test

Show status with `ibstat`. Test RDMA with `perftest`.

If you find tools like `perftest` does not work, it may releated to
- https://gist.github.com/zshi-redhat/c7cfe9e0be63f0330952a28792acff2b
- Limit on `memlock`, see below for solution.

### Disable `memlock` limit

IB tools may fail to allocate memory if memlock limit is too small.
To show current `memlock` limit, use

```bash
sudo systemctl show systemd-nspawn@<machine-name> --property LimitMEMLOCK
```

To disable limit, use

```bash
sudo systemctl edit systemd-nspawn@<machine-name>
```

And add `LimitMEMLOCK=infinity` to `[Service]` section, then restart your container.

## Troubleshooting

### No color in terminal

See Arch wiki for "broken colors" problem.

Create file `/etc/systemd/system/container-getty@.service.d/term.conf` **in container** with following contents:

```conf
[Service]
Environment=TERM=xterm-256color
```
