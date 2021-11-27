# Tuning

## Enable / Disable SMT (HyperThreading)

From <https://docs.kernelcare.com/how-to/>

```bash
# Check the SMT state
cat /sys/devices/system/cpu/smt/active

#Enable SMT
echo on > /sys/devices/system/cpu/smt/control

#Disable SMT
echo off > /sys/devices/system/cpu/smt/control
```
