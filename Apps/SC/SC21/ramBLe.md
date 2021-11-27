# ramBLe

## turn off hyperthreading

```Bash
sudo su
echo off > /sys/devices/system/cpu/smt/control
```

/home/opc/ramBLe

boost 1.70.0 & mvapich2.3.3

## turn off hyperthreading

```Bash
sudo su
echo off > /sys/devices/system/cpu/smt/control
```

## Gdrive

```YAML
wget https://github.com/prasmussen/gdrive/releases/download/2.1.1/gdrive_2.1.1_linux_amd64.tar.gz
tar -zxvf gdrive_2.1.1_linux_amd64.tar.gz

wget https://forensics.cert.org/cert-forensics-tools-release-el7.rpm
sudo rpm -Uvh cert-forensics-tools-release*rpm
sudo yum --enablerepo=forensics install musl-libc -y

```

## init env values

```Bash
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/nfs/cluster/boost_1_70_0/stage/lib

source /home/opc/ramBLe/env.sh
```

## mpi

```Bash
source /opt/hpcx-v2.8.1-gcc-MLNX_OFED_LINUX-5.2-2.2.0.0-redhat7.9-x86_64/hpcx-init.sh
hpcx_load

```

```Bash
mpirun -np 4 --display-map --map-by node -x MXM_RDMA_PORTS=mlx5_0:1 -mca btl_openib_if_include mlx5_0:1 
```

## run

```Bash
mpirun -np 144 --display-map --hostfile hostfiles  -x MXM_RDMA_PORTS=mlx5_0:1 -mca btl_openib_if_include mlx5_0:1  -x UCX_NET_DEVICES=mlx5_0:1  ./ramble -f test/coronary.csv -n 6 -m 1841 -d -o test/coronary.dot
```

```Bash
[opc@inst-dahrf-splendid-walrus ramBLe]$ cat hostfiles
hpc-node-1 slots=36
hpc-node-2 slots=36
hpc-node-3 slots=36
hpc-node-4 slots=36
```

```Bash
tab is $'\t'
```

## python run experinment

at `/nfs/cluster/ramBle_hpcg`

```Bash
python common/scripts/ramble_experiments.py \
-p 16 -r 1 -a gs -d /nfs/scratch/C1_discretized.tsv -s '\t' -v \
--results result\c1.csv
```

```Bash
mpirun -np 144 --display-map --hostfile hostfiles  -x MXM_RDMA_PORTS=mlx5_0:1 -mca btl_openib_if_include mlx5_0:1  -x UCX_NET_DEVICES=mlx5_0:1  ./ramble -f /nfs/scratch/C1_discretized.tsv -m 29150 -n 5164  -s $'\t' -v -i -d -o test/c1.dot
```

```Bash
mpirun -np 1 \
--display-map \
--hostfile hostfiles \
-x MXM_RDMA_PORTS=mlx5_0:1 \
-mca btl_openib_if_include mlx5_0:1 \
-x UCX_NET_DEVICES=mlx5_0:1 \
./ramble -f /nfs/scratch/C1_discretized.tsv -s $'\t' \
-n 29150 -m 5164 \
 -c -v -i -d -o test/c1_2.dot >> result/hp_1
```

```Bash
mpirun -np 144 \
--hostfile hostfiles \
-x MXM_RDMA_PORTS=mlx5_0:1 \
-mca btl_openib_if_include mlx5_0:1 \
-x UCX_NET_DEVICES=mlx5_0:1 \
./ramble -f test/coronary.csv -s ',' -n 6 -m 1841 -d -o test/coronary.dot
```

## Auto Run script

[murez/SC21_SCript/ramble](https://github.com/murez/SC21_SCripts/tree/main/ramBLe)

## Gdrive

```Bash
gdrive download 1UdrvrUPBQRjQafeOn5gHENz9wCrOrX-F    # ramBLe_hpcx.tar.gz
gdrive download 1QmW1RF6mvnepQ3hawMNK46MoDRNq8YGx    # boost_1_70_0_compiled.tar.gz

```



## Install

### Lib

[Boost](../../../Libs/Boost.md)

just add the code into `SConstruct` to tell `scons` where is the boost lib is.

```python
libPaths.append("/opt/spack/opt/spack/linux-debian10-zen2/gcc-10.2.0/boost-1.70.0-m4ttgcfqixwe22z5kz7bpp7mbqdspdbg/lib")
cppPaths.append("/opt/spack/opt/spack/linux-debian10-zen2/gcc-10.2.0/boost-1.70.0-m4ttgcfqixwe22z5kz7bpp7mbqdspdbg/include")
```

