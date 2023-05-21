---
title: check-list
description: 
published: true
date: 2022-03-21T03:20:17.098Z
tags: 
editor: markdown
dateCreated: 2022-03-21T02:21:56.023Z
---

# SGX Explained

- [x] ifconfig ib0 192.168.*
- [x] change ~/.bashrc for (non-)header conditional setup.
```bash
if [ -f /dev/ ]; then #头节点
setup eth .xxx ib() .xxx
fi
```
- [x] 100 机器 nfs
- [x] slurm 启动开始跑，机器 one by one启动，任务复用、
- [x] 脚本allocate 起停 可以轮流睡觉 轮流slurm
- [ ] MIG 启动两套命令/ rmmod nvdia*
- [x] Prometheus + Grafana for all chassis

## Performance
```
echo 2 > /proc/sys/vm/overcommit_memory
ulimit -a ulimited
echo performance > /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor
[ -f "/shared/opt/home/q-e" ] sudo mount 10.0.0.8:/mnt/exports/shared/home /shared/home
...
```
