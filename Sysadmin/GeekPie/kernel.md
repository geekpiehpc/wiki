# Kernel

We use a custom kernel with NOHZ support enabled.

## Build Kernel on Debian/Ubuntu

To build kernel, refer to [Chapter 4. Common kernel-related tasks (pages.debian.net)](https://kernel-team.pages.debian.net/kernel-handbook/ch-common-tasks.html#s-common-official).

Current kernel config is at `/usr/src/linux-headers-$(uname -r)/.config`.

> [Kernel/BuildYourOwnKernel - Ubuntu Wiki](https://wiki.ubuntu.com/Kernel/BuildYourOwnKernel) and [BuildADebianKernelPackage - Debian Wiki](https://wiki.debian.org/BuildADebianKernelPackage) are obsolete, do not use them.

If you don't want to use module signing:

```bash
scripts/config --disable MODULE_SIG
scripts/config --disable SYSTEM_TRUSTED_KEYS
```

Also consider disable debug info:

```bash
scripts/config --disable DEBUG_INFO
```
