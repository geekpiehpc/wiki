<!-- TITLE: Environment Installation -->
<!-- SUBTITLE: Installation records for environment dependencies -->

# 环境安装记录

环境安装方式 + 目录树位置

## 安装目录树结构

```
|- /opt/
    |- openmpi/
        |- 4.0
        |- 3.1
        |- ...
    |- mpich/
    |- intel/    # Intel 全家福
    |- blas/
    |- gcc/
    |- cuda/     # Nvidia CUDA
    |- pgi/      # CUDA PGi Edition
    |- netcdf/
        |- netcdf-c/
        |- netcdf-fort/
    |- pnetcdf
```

> 编译安装基本六连：
> 
> `$ wget [SOURCE_URL]`
> `$ tar zxvf openmpi-4.0.0.tar.gz`
> `$ cd openmpi-4.0.0/`
> `$ ./configure --prefix=‘/opt/mpi/openmpi/4.0’   # 注意规划好位置`
> `$ make -j8`
> `$ make install`

## 包管理系统
>[spack](https://spack.io/)
>Environment Modules
二选一， `spack` 基本上就是对系统级的`modules`的高层API，从ASC20开始，我们开始使用`spack`。为保证目录树nfs共享结构，把`spack` 放在/opt 目录下。

>$ git clone https://github.com/spack/spack.git
>$ cd spack/bin
>$ ./spack install libelf # test
>$ echo "export PATH=$PATH:/opt/spack/bin" >> ~/.bashrc
>$ echo ". /opt/spack/share/spack/setup-env.sh" >> ~/.bashrc
>$ bash

依赖以及版本spec
> $ spack install intel^gcc@9

在测试时使用
> $ spack load intel^gcc@9
也可以`module avail` 后查看需要load 的环境`module load intel`

添加新的编译器，在自己编译安装好一个编译器并在`PATH`中可以找到的时候，可以使用`spack find`命令，之后就可以用这个编译器`@intel`来编译新的编译器了。

特殊注意，在编译安装`mpi`,`omp`过程中一定要开启 --with-rdma 选项以支持Infiniband.

## 编译器

1. **gcc** (包含 **gfortran**) - Version 7.4 + 5.5 + 4.9.4 + 4.4.7
    - `CentOS` 自带的gcc版本过老，可以使用 `scl enable devtoolset-9 bash`以支持最新gcc特性。
    - 7.4：[ftp://ftp.gnu.org/gnu/gcc/gcc-7.4.0/gcc-7.4.0.tar.gz](ftp://ftp.gnu.org/gnu/gcc/gcc-7.4.0/gcc-7.4.0.tar.gz)
    - 5.5：[ftp://ftp.gnu.org/gnu/gcc/gcc-5.5.0/gcc-5.5.0.tar.gz](ftp://ftp.gnu.org/gnu/gcc/gcc-5.5.0/gcc-5.5.0.tar.gz)
    - 4.4.7：[ftp://ftp.gnu.org/gnu/gcc/gcc-4.4.7/gcc-4.4.7.tar.gz](ftp://ftp.gnu.org/gnu/gcc/gcc-4.4.7/gcc-4.4.7.tar.gz)
2. **icc** & **ifort**：包含于 *Intel Parallel Studio XE* 中

## Intel Parallel Studio XE 全家桶

**Parallel Studio XE**：按照 [This Procedure](https://www.slothparadise.com/how-to-setup-the-intel-compilers-on-a-cluster) 获取和安装，19-20 授权如下

- 序列号 S4ZD-MMZJXJ96 (若失效，可以前往英特尔官网申请，在下方register center，若为spack 安装只需在安装过程中输入即可)
- URL：[http://registrationcenter-download.intel.com/akdlm/irc_nas/tec/15088/parallel_studio_xe_2019_update2_cluster_edition.tgz](http://registrationcenter-download.intel.com/akdlm/irc_nas/tec/15088/parallel_studio_xe_2019_update2_cluster_edition.tgz)
- LICENSE：官网 Registration Center 下载后传至 Server

> `icc` `ifort` `mkl` `IntelMPI` 均包含于 Parallel Studio XE 中
> `spack install intel`

由于cuda只支持编译它的编译器头文件在gcc-7标准以前，所以建议使用intel@18.0.3
## MPI

1. **OpenMPI** - Version 4.0 + 3.1 + 3.0 + 2.1
    - 4.0：[https://download.open-mpi.org/release/open-mpi/v4.0/openmpi-4.0.0.tar.gz](https://download.open-mpi.org/release/open-mpi/v4.0/openmpi-4.0.0.tar.gz)
    - 3.1：[https://download.open-mpi.org/release/open-mpi/v3.1/openmpi-3.1.3.tar.gz](https://download.open-mpi.org/release/open-mpi/v3.1/openmpi-3.1.3.tar.gz)
    - 3.0：[https://download.open-mpi.org/release/open-mpi/v3.0/openmpi-3.0.3.tar.gz](https://download.open-mpi.org/release/open-mpi/v3.0/openmpi-3.0.3.tar.gz)
    - 2.1：[https://download.open-mpi.org/release/open-mpi/v2.1/openmpi-2.1.6.tar.gz](https://download.open-mpi.org/release/open-mpi/v2.1/openmpi-2.1.6.tar.gz)
2. **MPICH** - Version 3.3 + 3.2.1
    - 3.3：[http://www.mpich.org/static/downloads/3.3/mpich-3.3.tar.gz](http://www.mpich.org/static/downloads/3.3/mpich-3.3.tar.gz)
    - 3.2.1：[http://www.mpich.org/static/downloads/3.2.1/mpich-3.2.1.tar.gz](http://www.mpich.org/static/downloads/3.2.1/mpich-3.2.1.tar.gz)
3. **IntelMPI**：包含于 *Intel Parallel Studio XE* 中

## Nvidia CUDA

- **CUDA Toolkit**：`spack install cuda@10.2` 以支持不同版本。
- **PGi Edition**：`spack install pgi@19.10`

## Math Libraries

1. **MKL**：包含于 *Intel Parallel Studio XE* 中
2. **OpenBLAS**：`spack install openblas`

## NetCDF I/O
用于ASC19 的IO500题目中。
