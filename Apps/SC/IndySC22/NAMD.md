---
title: NAMD
description: 
published: true
date: 2022-12-04T11:25:30.457Z
tags: 
editor: markdown
dateCreated: 2022-12-04T11:24:48.762Z
---

# Author
@Zecheng Li
# Tutorial

**Video**: [https://drive.google.com/file/d/1c2bD3gZw5ZeJS81i1uXY6eAXf70t8bZo/view](https://drive.google.com/file/d/1c2bD3gZw5ZeJS81i1uXY6eAXf70t8bZo/view)

**NAMD 2.14 User’s Guide**: [https://www.ks.uiuc.edu/Research/namd/2.14/ug/](https://www.ks.uiuc.edu/Research/namd/2.14/ug/)

**NAMD Tutorial**: [http://www.ks.uiuc.edu/Training/Tutorials/namd/namd-tutorial-unix-html/index.html](http://www.ks.uiuc.edu/Training/Tutorials/namd/namd-tutorial-unix-html/index.html)

VMD User’s Guide: [https://www.ks.uiuc.edu/Research/vmd/current/ug/](https://www.ks.uiuc.edu/Research/vmd/current/ug/)

VMD Tutorial: [https://www.ks.uiuc.edu/Training/Tutorials/vmd/tutorial-html/](https://www.ks.uiuc.edu/Training/Tutorials/vmd/tutorial-html/)

# Building

We use spack as the package manager. To build a simple version **without** MPI support: 

```
spack install namd
```

In our competition, we need to support multiple nodes, so we choose to install charmpp with MPI backend and SMP enabled. The TCL interface is included for parsing the input file. The below command will depend on the OpenMPI with ucx support. 

```
spack install -v namd%gcc interface=tcl ^charmpp backend=mpi ^openmpi fabrics=ucx
```

We could also use a pure MPI version. But unlike some other applications, NAMD optimized its performance for multi-threading, so the SMP version is usually faster than MPI when a single node has multiple cores. When running, we should use more communication threads (one per numa) for larger-scale jobs. 

## Tuning performance

We could try different compilers to build NAMD. The performance critical part of NAMD is the force calculation implemented in the source of NAMD itself (instead of in some math libraries), so compiler optimization is crucial for the performance. 

We could try different compilers: (oneapi is not supported by charm++ that NAMD 2.14 depends on)

```
spack install -v namd%intel interface=tcl ^charmpp%intel backend=mpi ^openmpi fabrics=ucx
spack install -v namd%nvhpc interface=tcl ^charmpp%nvhpc backend=mpi ^openmpi fabrics=ucx
spack install -v namd%clang interface=tcl ^charmpp%clang backend=mpi ^openmpi fabrics=ucx
```

As you may find, Intel compiler might yield better performance than gcc, and NVHPC and clang are extremely slow. Don't worry. Have a look at the build scripts of NAMD. 

```
spack edit namd

if self.spec.satisfies("^charmpp@:6.10.1"):
 optims_opts = {
 "gcc": m64
 + "-O3 -fexpensive-optimizations \
 -ffast-math -lpthread "
 + archopt,
 "intel": "-O2 -ip -qopenmp-simd" + archopt,
 "aocc": m64
 + "-O3 -ffp-contract=fast -ffast-math \
 -fopenmp "
 + archopt,
 }
else:
 optims_opts = {
 "gcc": m64
 + "-O3 -fexpensive-optimizations \
 -ffast-math -lpthread "
 + archopt,
 "intel": "-O2 -ip " + archopt,
 "aocc": m64
 + "-O3 -ffp-contract=fast \
 -ffast-math "
 + archopt,
 }
```

It did not set the optimization flags for clang and NVHPC. We could add them by ourselves. Below is an example; you should try different flags based on your architecture. 

```
"clang": m64 + "-O3 -ffp-contract=fast -ffast-math -mprefer-vector-width=256 " + archopt,
"nvhpc": m64 + "-O3 -fast " + archopt,
```

Then we could build NAMD with clang and NVHPC. Surprisingly, clang is faster than Intel compiler on our machine (Haswell). 

From the [charm++ documentation](https://charm.readthedocs.io/en/latest/charm++/manual.html), we could find that we can compile different load balancer modules into NAMD with different flags, but the default spack build script did not include them. Having a suitable load balancer for your architecture and interconnect is important. Some load balancers might cause a compile error since multiple definitions. 

> -module TreeLB -module RecBipartLB ... 
> links the listed LB modules into an application, which can then be used at runtime via the +balancer option.

```
spack edit namd

# in function def edit(self, spec, prefix): add line
opts.extend(["-module CentralLB -module DistributedLB"])
```

From our experience, the default load balancer, the CentralLB, and the DistributedLB should be chosen based on the input and the architecture. It will bring about a 5-10% performance difference. You can also experiment with other load balancers or even write your own load balancer (not that hard!).


There are also some other flags that could be tuned. Since our machine does not support AVX512, we did not try the Intel-optimized AVX512 blocking version of NAMD. 

# Assignment

1. Quick Start & Homework: [https://gitlab.msu.edu/vermaaslab/indysccnamd/-/tree/main](https://gitlab.msu.edu/vermaaslab/indysccnamd/-/tree/main)

2. Running Command:

   ```
   # One per node with SMP
   mpirun -np 20 -hostfile ~/work/host20 -bind-to core -map-by ppr:1:node -x PATH namd2 +ppn 19 +pemap 1-19 +commap 0 run.namd

   ​# One per NUMA with SMP (You should check your NUMA topology first)
   mpirun -np 40 -hostfile ~/work/host20 --bind-to core --map-by ppr:2:node  -x PATH  namd2   +ppn 9 +pemap 1-4,10-14,5-9,15-19 +commap 0,5 ./run.namd

   # Run with 19 replicas
   mpirun -np 19 -hostfile ~/work/host20 --bind-to core --map-by ppr:1:node  -x PATH namd2 +replicas 19 +balancer DistributedLB +ppn 20 +pemap 0-19 +commap 0 +stdout output/out.%d.log ./replicaconfig.namd
   ```
   Here we oversubscribe the cores. Since core 0 is lightly loaded when communication is not heavy, we can also assign it to computation.
   Note that in above commands, we always put core 0 or core 5 to communication. This is because we have set most of the interrupt affinity to core 0 and core 5, using them could get better performance on both communication and computation. (it will be up to 5% slower if you use other cores)

3. MPI Reference: [https://www.ks.uiuc.edu/Research/namd/2.9/ug/node87.html](https://www.ks.uiuc.edu/Research/namd/2.9/ug/node87.html)
4. Our submission: [https://drive.google.com/file/d/1HqxWP6YJIr06wz6ANMHog3v59HnhV7T2/view?usp=share_link](https://drive.google.com/file/d/1HqxWP6YJIr06wz6ANMHog3v59HnhV7T2/view?usp=share_link)



# Final

1. Problem Set: [https://drive.google.com/file/d/1zyWpv-bfN2uzke7RqnpS8PI6Q-AQFtKb/view?usp=share_link](https://drive.google.com/file/d/1zyWpv-bfN2uzke7RqnpS8PI6Q-AQFtKb/view?usp=share_link)

2. Our Submission: [https://drive.google.com/drive/folders/1dpVS6027vJTsbxlOjfMEGdftOfQyCmlO?usp=share_link](https://drive.google.com/drive/folders/1dpVS6027vJTsbxlOjfMEGdftOfQyCmlO?usp=share_link)

   

# Experience

NAMD配置文件参数介绍：

1. NAMD configuration parameters： [https://www.ks.uiuc.edu/Research/namd/2.9/ug/node12.html](https://www.ks.uiuc.edu/Research/namd/2.9/ug/node12.html)
2. Non-bonded Interaction & Parameters: [https://www.ks.uiuc.edu/Research/namd/2.10b2/ug/node23.html](https://www.ks.uiuc.edu/Research/namd/2.10b2/ug/node23.html)

调参方法：

1. 整体思路：在不跑崩的范围内，timestep和nonbondedFreq的乘积、timestep和fullElectFrequency的乘积尽可能大
2.  输出间隔也对性能有影响，调小后约能提升5-10%性能

2. Reference：
   1. [https://www.ks.uiuc.edu/Research/namd/wiki/index.cgi?NamdPerformanceTuning](https://www.ks.uiuc.edu/Research/namd/wiki/index.cgi?NamdPerformanceTuning)
   2. [https://www.ks.uiuc.edu/Research/namd/cvs/ug/node95.html](https://www.ks.uiuc.edu/Research/namd/cvs/ug/node95.html)