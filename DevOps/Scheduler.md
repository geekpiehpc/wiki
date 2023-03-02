---
title: Scheduler
description: 
published: true
date: 2022-03-21T03:19:09.731Z
tags: 
editor: markdown
dateCreated: 2022-03-21T02:20:11.334Z
---

# PBS

PBS 全称为 [Portable Batch System](https://en.wikipedia.org/wiki/Portable_Batch_System)，可以用来控制多个计算机上的任务。

常见使用方式如下，`qcmd` 为任意 PBS 命令:

```bash
# Name for the job
qcmd -N ramBLe_128

# Name of destination queue
qcmd -q GeekPie_CPU

# Required resources
qcmd -l nodes=4:ppn=32:amd
qcmd -l walltime=00:10:00

# Redirect stdout/stderr
qcmd -o /public/home/geekpie2/ramble-amd/ramBLe/submit/pbs-com-single-${PBS_JOBID}.out
qcmd -e /public/home/geekpie2/ramble-amd/ramBLe/submit/pbs-com-single-${PBS_JOBID}.err
```

## 常用命令

- **qsub**: 提交任务或启动交互式 Shell
- **qstat**: 查看任务状态
  - 如果需要显示详细信息，可以使用 **-f** 参数
  - 如果需要查看队列状态，可以使用 **-Q** 参数，后接队列名称
  - 例如: `qstat -Qf GeekPie-CPU`
- **qdel**: 删除任务

## 常用参数

| 参数 | 值 | 说明 |
| --- | --- | --- |
| -q | 队列、服务器，或服务器上的队列 | 设置执行任务的主体 |
| -N | 任务名称 | 设置任务名称 |
| -l | 资源列表，使用逗号分隔 | 设置需要的资源，该命令可指定多次 |
| -o | 输出文件 | stdout 内容将被重定向到该文件中，推荐使用绝对路径 |
| -e | 错误文件 | stderr 内容将被重定向到该文件中，推荐使用绝对路径 |

## 参考文档

- [Quick Tutorial for Portable Batch System](https://albertsk.files.wordpress.com/2011/12/pbs.pdf)

# Slurm
本超算使用的是 Slurm，详细的配置可见 [配合某戏精使用的 slurm 踩坑日记](https://www.victoryang00.cn/wordpress/2021/01/31/pei-he-mou-xi-jing-shi-yong-de-slurm-cai-keng-ri-j/)。
