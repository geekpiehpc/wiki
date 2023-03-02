---
title: HPL hpl.dat Config
description: Ancestral hpl.dat config file
published: true
date: 2022-03-21T03:18:55.817Z
tags: 
editor: markdown
dateCreated: 2022-03-21T02:19:49.492Z
---

# HPL .dat config file
## ASC18
The following is the HPL `.dat` configuration file template from ASC18.

```
HPLinpack benchmark input file
Innovative Computing Laboratory, University of Tennessee
HPL.out      output file name (if any)
6            device out (6=stdout,7=stderr,file)
1            # of problems sizes (N)
67200 65280 62976 65280 96000 65280 38400 96000 102400 168960 153600 76800   142848 153600 142848 124416 96256 142848 124416 115200 110592 96256 Ns
1             # of NBs
384 768 384 768 1024 768 896 768 1024 512 384 640 768 896 960 1024 1152 1280 384 640 960 768 640 256  960 512 768 1152         NBs
0            PMAP process mapping (0=Row-,1=Column-major)
1            # of process grids (P x Q)
2 1 2 1        Ps
1 2 2 4        Qs
16.0         threshold
1            # of panel fact
0 1 2        PFACTs (0=left, 1=Crout, 2=Right)
1            # of recursive stopping criterium
2 8          NBMINs (>= 1)
1            # of panels in recursion
2            NDIVs
1            # of recursive panel fact.
0 1 2        RFACTs (0=left, 1=Crout, 2=Right)
1            # of broadcast
2 0 2          BCASTs (0=1rg,1=1rM,2=2rg,3=2rM,4=Lng,5=LnM)
1            # of lookahead depth
0            DEPTHs (>=0)
1            SWAP (0=bin-exch,1=long,2=mix)
192          swapping threshold
1            L1 in (0=transposed,1=no-transposed) form
0            U  in (0=transposed,1=no-transposed) form
1            Equilibration (0=no,1=yes)
8            memory alignment in double (> 0)
```

## ASC20
The following is the HPL `.dat` configuration file template from ASC20.
Machine Spec : 8 Tesla V100

```
HPLinpack benchmark input file
Innovative Computing Laboratory, University of Tennessee
HPL.out      output file name (if any)
6            device out (6=stdout,7=stderr,file)
2            # of problems sizes (N)
175104 178176 165888 168960 172032 175104 Ns
2             # of NBs
384 256 128 256 384 192 288 320 384 384 768 1024 768 896 768 1024 512 384 640 768 896 960 1024 1152 1280 384 640 960 768 640 256  960 512 768 1152         NBs
0            PMAP process mapping (0=Row-,1=Column-major)
1            # of process grids (P x Q)
4 2 8 1 2 1        Ps
4 8 2 2 4        Qs
16.0         threshold
1            # of panel fact
0 1 2        PFACTs (0=left, 1=Crout, 2=Right)
1            # of recursive stopping criterium
2 8          NBMINs (>= 1)
1            # of panels in recursion
2            NDIVs
1            # of recursive panel fact.
0 1 2        RFACTs (0=left, 1=Crout, 2=Right)
1            # of broadcast
2 0 2          BCASTs (0=1rg,1=1rM,2=2rg,3=2rM,4=Lng,5=LnM)
1            # of lookahead depth
0            DEPTHs (>=0)
1            SWAP (0=bin-exch,1=long,2=mix)
192          swapping threshold
1            L1 in (0=transposed,1=no-transposed) form
0            U  in (0=transposed,1=no-transposed) form
1            Equilibration (0=no,1=yes)
8            memory alignment in double (> 0)
```

## SC21
The following is the HPL `.dat` configuration file template from SC20.
Machine Spec : 8 Tesla A100 

```
HPLinpack benchmark input file
Innovative Computing Laboratory, University of Tennessee
HPL.out      output file name (if any)
6            device out (6=stdout,7=stderr,file)
2            # of problems sizes (N)
346122 348122 352122 Ns
2             # of NBs
384 256 128 NBs
0            PMAP process mapping (0=Row-,1=Column-major)
1            # of process grids (P x Q)
4 2 8 1 2 1        Ps
4 8 2 2 4        Qs
16.0         threshold
1            # of panel fact
0 1 2        PFACTs (0=left, 1=Crout, 2=Right)
1            # of recursive stopping criterium
2 8          NBMINs (>= 1)
1            # of panels in recursion
2            NDIVs
1            # of recursive panel fact.
0 1 2        RFACTs (0=left, 1=Crout, 2=Right)
1            # of broadcast
2 0 2          BCASTs (0=1rg,1=1rM,2=2rg,3=2rM,4=Lng,5=LnM)
1            # of lookahead depth
0            DEPTHs (>=0)
1            SWAP (0=bin-exch,1=long,2=mix)
192          swapping threshold
1            L1 in (0=transposed,1=no-transposed) form
0            U  in (0=transposed,1=no-transposed) form
1            Equilibration (0=no,1=yes)
8            memory alignment in double (> 0)
```

### Binder
```
#!/bin/bash
cd $1
# Global settings

export UCX_RNDV_SCHEME=put_zcopy
export UCX_IB_PCI_RELAXED_ORDERING=on
export UCX_MEMTYPE_CACHE=n
export UCX_MAX_RNDV_RAILS=1
export UCX_RNDV_THRESH=8192

APP="$2"
me=`hostname`
lrank=$OMPI_COMM_WORLD_LOCAL_RANK

case ${lrank} in
0)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP"
    source ../source.sh
    export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
1)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP"
    export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
2)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
3)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
4)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
5)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
6)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;
7)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;
8)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP"
   source ../source.sh
    export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
9)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP"
    export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
10)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
11)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
12)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
13)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
14)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;
15)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;
16)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP"
    source ../source.sh
    export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
17)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP"
    export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
18)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
19)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
20)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
21)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
22)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;
23)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;

24)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP"
    source ../source.sh
    export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
25)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP"
    export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
26)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
27)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
28)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
29)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
30)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;
31)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;
32)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP"
    source ../source.sh
    export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
33)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP"
    export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
34)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
35)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
36)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
37)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
38)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;
39)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;
40)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP"
    source ../source.sh
    export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
41)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP"
    export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
42)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
43)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
44)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
45)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
46)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;
47)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;
48)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP"
    source ../source.sh
    export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
49)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP"
    export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
50)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
51)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
52)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
53)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
54)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;
55)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;
56)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP"
    source ../source.sh
    export CUDA_VISIBLE_DEVICES=2; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
57)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP"
    export CUDA_VISIBLE_DEVICES=3; numactl --cpunodebind=0 taskset -c 0-23 $APP
  ;;
58)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=7; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
59)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP"
    export CUDA_VISIBLE_DEVICES=6; numactl --cpunodebind=2  taskset -c 48-71 $APP
  ;;
60)
#ldd $APP
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=0; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
61)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP"
    export CUDA_VISIBLE_DEVICES=1; numactl --cpunodebind=1 taskset -c 24-47 $APP
  ;;
62)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=5; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;
63)
echo "host=$me rank= $OMPI_COMM_WORLD_RANK lrank = $lrank cores=$CPU_CORES_PER_RANK bin=$APP"
  #set GPU and CPU affinity of local rank
    echo "export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP"
    export CUDA_VISIBLE_DEVICES=4; numactl --cpunodebind=3 taskset -c 72-95 $APP
  ;;

esac
```