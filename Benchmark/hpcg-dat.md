---
title: Hpcg Dat
description: A quick summary of Hpcg Dat
published: true
date: 2022-03-21T03:18:52.901Z
tags: 
editor: markdown
dateCreated: 2022-03-21T02:19:45.208Z
---

# .dat Specs
## ASC20
```
HPCG benchmark input file
Sandia National Laboratories; University of Tennessee, Knoxville
384 256 256
60
```
### how to run
```bash
export PATH=/opt/nonspack/hpcx-v2.8.1-gcc-MLNX_OFED_LINUX-5.2-2.2.0.0-ubuntu18.04-x86_64/ompi/bin:/opt/nonspack/hpcx-v2.8.1-gcc-MLNX_OFED_LINUX-5.2-2.2.0.0-ubuntu18.04-x86_64/ompi/tests/osu-micro-benchmarks-5.6.2/:$PATH
export LD_LIBRARY_PATH=/opt/nonspack/hpcx-v2.8.1-gcc-MLNX_OFED_LINUX-5.2-2.2.0.0-ubuntu18.04-x86_64/ompi/lib:$LD_LIBRARY_PATH
source /etc/profile.d/modules.sh
module use /opt/nonspack/hpcx-v2.8.1-gcc-MLNX_OFED_LINUX-5.2-2.2.0.0-ubuntu18.04-x86_64/modulefiles
module load hpcx
```

```bash
mpirun --allow-run-as-root --hostfile host2_gpu4 --mca pml_base_verbose 100     --mca btl_base_verbose 100     --mca pml ucx -x UCX_NET_DEVICES=mlx5_0:1     --mca orte_base_help_aggregate=0     -x xhpcg-3.1_cuda-11_ompi-4.0_sm_60_sm70_sm80 
```
## SC21
```
HPCG benchmark input file
Sandia National Laboratories; University of Tennessee, Knoxville
256 256 512
1800
```
### how to run
see [binder.sh](hpl-dat.md#Binder)