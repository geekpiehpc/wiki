# Tuning

## Enable / Disable SMT (HyperThreading)

[Simultaneous multithreading (SMT)](https://en.wikipedia.org/wiki/Simultaneous_multithreading) is a technique for improving the overall efficiency of superscalar CPUs with hardware multithreading.

``` bash
# From https://docs.kernelcare.com/how-to/

# Check the SMT state
cat /sys/devices/system/cpu/smt/active

#Enable SMT
echo on > /sys/devices/system/cpu/smt/control

#Disable SMT
echo off > /sys/devices/system/cpu/smt/control
```

## CPU Isolation

When kernel is booted with `nohz_full=1-127` set, CPU 1-127 are isolated. Refer [CPU Isolation – Nohz\_full – by SUSE Labs \(part 3\) \| SUSE Communities](https://www.suse.com/c/cpu-isolation-nohz_full-part-3/) for more details.

A full list of kernel parameters is available at <https://www.kernel.org/doc/html/latest/admin-guide/kernel-parameters.html>.
