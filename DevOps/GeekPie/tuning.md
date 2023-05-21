---
title: tuning
description: 
published: true
date: 2022-03-25T10:43:04.295Z
tags: 
editor: markdown
dateCreated: 2022-03-21T02:22:27.080Z
---

# Tuning

## Enable / Disable SMT (HyperThreading)

[Simultaneous multithreading (SMT)](https://en.wikipedia.org/wiki/Simultaneous_multithreading) is a technique for improving the overall efficiency of superscalar CPUs with hardware multithreading.

```bash
# From https://docs.kernelcare.com/how-to/

# Check the SMT state
cat /sys/devices/system/cpu/smt/active

#Enable SMT
echo on > /sys/devices/system/cpu/smt/control

#Disable SMT
echo off > /sys/devices/system/cpu/smt/control
```

## Tick-free CPU

When kernel is booted with `nohz_full=1-127` set, CPU 1-127 are isolated. Refer [CPU Isolation - Nohz_full - by SUSE Labs (part 3) | SUSE Communities](https://www.suse.com/c/cpu-isolation-nohz_full-part-3/) for more details.

Also see:

* [3.13. Isolating CPUs Using tuned-profiles-realtime Red Hat Enterprise Linux for Real Time 7 | Red Hat Customer Portal](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux_for_real_time/7/html/tuning_guide/isolating_cpus_using_tuned-profiles-realtime).
* [NO_HZ: Reducing Scheduling-Clock Ticks](https://www.kernel.org/doc/Documentation/timers/NO_HZ.txt)

A full list of kernel parameters is available at [https://www.kernel.org/doc/html/latest/admin-guide/kernel-parameters.html](https://www.kernel.org/doc/html/latest/admin-guide/kernel-parameters.html).

## Set `kernel.yama.ptrace_scope` to 0

For temporary applying, use the following command
```bash
sudo sysctl -w kernel.yama.ptrace_scope=0
```

For permanent, change `/etc/sysctl.d/10-ptrace.conf`.

For documentation, see [The Linux kernel user’s and administrator’s guide » Linux Security Module Usage - Yama](https://www.kernel.org/doc/html/v4.16/admin-guide/LSM/Yama.html?msclkid=a4bbb405ac2711eca35edc9bacd46306).

