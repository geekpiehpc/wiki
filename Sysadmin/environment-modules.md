<!-- TITLE: Environment Modules -->
<!-- SUBTITLE: Environment Module Management & Backups -->

# Modulefile & Spack 记录

Environment Module: Modulefiles 目录树结构 + 备份

## Modulefiles 目录树结构 （Deprecated）

```bash
|- /opt/modulefiles    # 路径并非完全按照冲突关系组织，modulefile 中冲突关系要注意
    |- mpi/
        |- openmpi/
            |- 4.0
            |- 3.1
            |- ...
        |- mpich/
        |- intelmpi/
    |- math/
        |- mkl/
        |- blas/
    |- compilers/
        |- gcc/
        |- icc/
        |- ifort/
    |- cuda/
        |- nvidia/
        |- pgi/
    |- netcdf/
        |- pnetcdf/
        |- netcdf-c/
        |- netcdf-fort/
```

## Spack 简单教程
```c
❯ spack info gcc
AutotoolsPackage:   gcc

Description:
    The GNU Compiler Collection includes front ends for C, C++, Objective-C,
    Fortran, Ada, and Go, as well as libraries for these languages.

Homepage: https://gcc.gnu.org

Maintainers: @michaelkuhn @alalazo

Externally Detectable:
    True (version, variants)

Tags:
    None

Preferred version:
    11.2.0    https://ftpmirror.gnu.org/gcc/gcc-11.2.0/gcc-11.2.0.tar.xz

Safe versions:
    master    [git] git://gcc.gnu.org/git/gcc.git on branch master
    11.2.0    https://ftpmirror.gnu.org/gcc/gcc-11.2.0/gcc-11.2.0.tar.xz
    11.1.0    https://ftpmirror.gnu.org/gcc/gcc-11.1.0/gcc-11.1.0.tar.xz
    10.3.0    https://ftpmirror.gnu.org/gcc/gcc-10.3.0/gcc-10.3.0.tar.xz
    10.2.0    https://ftpmirror.gnu.org/gcc/gcc-10.2.0/gcc-10.2.0.tar.xz
    10.1.0    https://ftpmirror.gnu.org/gcc/gcc-10.1.0/gcc-10.1.0.tar.xz
    9.4.0     https://ftpmirror.gnu.org/gcc/gcc-9.4.0/gcc-9.4.0.tar.xz
    9.3.0     https://ftpmirror.gnu.org/gcc/gcc-9.3.0/gcc-9.3.0.tar.xz
    9.2.0     https://ftpmirror.gnu.org/gcc/gcc-9.2.0/gcc-9.2.0.tar.xz
    9.1.0     https://ftpmirror.gnu.org/gcc/gcc-9.1.0/gcc-9.1.0.tar.xz
    8.5.0     https://ftpmirror.gnu.org/gcc/gcc-8.5.0/gcc-8.5.0.tar.xz
    8.4.0     https://ftpmirror.gnu.org/gcc/gcc-8.4.0/gcc-8.4.0.tar.xz
    8.3.0     https://ftpmirror.gnu.org/gcc/gcc-8.3.0/gcc-8.3.0.tar.xz
    8.2.0     https://ftpmirror.gnu.org/gcc/gcc-8.2.0/gcc-8.2.0.tar.xz
    8.1.0     https://ftpmirror.gnu.org/gcc/gcc-8.1.0/gcc-8.1.0.tar.xz
    7.5.0     https://ftpmirror.gnu.org/gcc/gcc-7.5.0/gcc-7.5.0.tar.xz
    7.4.0     https://ftpmirror.gnu.org/gcc/gcc-7.4.0/gcc-7.4.0.tar.xz
    7.3.0     https://ftpmirror.gnu.org/gcc/gcc-7.3.0/gcc-7.3.0.tar.xz
    7.2.0     https://ftpmirror.gnu.org/gcc/gcc-7.2.0/gcc-7.2.0.tar.xz
    7.1.0     https://ftpmirror.gnu.org/gcc/gcc-7.1.0/gcc-7.1.0.tar.bz2
    6.5.0     https://ftpmirror.gnu.org/gcc/gcc-6.5.0/gcc-6.5.0.tar.bz2
    6.4.0     https://ftpmirror.gnu.org/gcc/gcc-6.4.0/gcc-6.4.0.tar.bz2
    6.3.0     https://ftpmirror.gnu.org/gcc/gcc-6.3.0/gcc-6.3.0.tar.bz2
    6.2.0     https://ftpmirror.gnu.org/gcc/gcc-6.2.0/gcc-6.2.0.tar.bz2
    6.1.0     https://ftpmirror.gnu.org/gcc/gcc-6.1.0/gcc-6.1.0.tar.bz2
    5.5.0     https://ftpmirror.gnu.org/gcc/gcc-5.5.0/gcc-5.5.0.tar.bz2
    5.4.0     https://ftpmirror.gnu.org/gcc/gcc-5.4.0/gcc-5.4.0.tar.bz2
    5.3.0     https://ftpmirror.gnu.org/gcc/gcc-5.3.0/gcc-5.3.0.tar.bz2
    5.2.0     https://ftpmirror.gnu.org/gcc/gcc-5.2.0/gcc-5.2.0.tar.bz2
    5.1.0     https://ftpmirror.gnu.org/gcc/gcc-5.1.0/gcc-5.1.0.tar.bz2
    4.9.4     https://ftpmirror.gnu.org/gcc/gcc-4.9.4/gcc-4.9.4.tar.bz2
    4.9.3     https://ftpmirror.gnu.org/gcc/gcc-4.9.3/gcc-4.9.3.tar.bz2
    4.9.2     https://ftpmirror.gnu.org/gcc/gcc-4.9.2/gcc-4.9.2.tar.bz2
    4.9.1     https://ftpmirror.gnu.org/gcc/gcc-4.9.1/gcc-4.9.1.tar.bz2
    4.8.5     https://ftpmirror.gnu.org/gcc/gcc-4.8.5/gcc-4.8.5.tar.bz2
    4.8.4     https://ftpmirror.gnu.org/gcc/gcc-4.8.4/gcc-4.8.4.tar.bz2
    4.7.4     https://ftpmirror.gnu.org/gcc/gcc-4.7.4/gcc-4.7.4.tar.bz2
    4.6.4     https://ftpmirror.gnu.org/gcc/gcc-4.6.4/gcc-4.6.4.tar.bz2
    4.5.4     https://ftpmirror.gnu.org/gcc/gcc-4.5.4/gcc-4.5.4.tar.bz2

Variants:
    Name [Default]               Allowed values          Description
    =========================    ====================    ===================================================

    binutils [off]               on, off                 Build via binutils
    bootstrap [on]               on, off                 Enable 3-stage bootstrap
    graphite [off]               on, off                 Enable Graphite loop optimizations (requires ISL)
    languages [c,c++,fortran]    ada, brig, c, c++,      Compilers and runtime libraries to build
                                 fortran, go, java,
                                 jit, lto, objc,
                                 obj-c++
    nvptx [off]                  on, off                 Target nvptx offloading to NVIDIA GPUs
    piclibs [off]                on, off                 Build PIC versions of libgfortran.a and libstdc++.a
    strip [off]                  on, off                 Strip executables to reduce installation size

Installation Phases:
    autoreconf    configure    build    install

Build Dependencies:
    binutils  cuda  diffutils  flex  gmp  gnat  iconv  isl  mpc  mpfr  zip  zlib  zstd

Link Dependencies:
    binutils  cuda  gmp  gnat  iconv  isl  mpc  mpfr  zlib  zstd

Run Dependencies:
    binutils

Virtual Packages:
    gcc@7: languages=go provides golang@:1.8
    gcc@6: languages=go provides golang@:1.6.1
    gcc@5: languages=go provides golang@:1.4
    gcc@4.9: languages=go provides golang@:1.2
    gcc@4.8.2: languages=go provides golang@:1.1.2
    gcc@4.8: languages=go provides golang@:1.1
    gcc@4.7.1: languages=go provides golang@:1
    gcc@4.6: languages=go provides golang
```

得到相关依赖，可以查看你现在如果安装 spack 提供的依赖

```c
❯ spack spec gcc  
Input spec
--------------------------------
gcc

Concretized
--------------------------------
gcc@11.2.0%apple-clang@12.0.5~binutils+bootstrap~graphite~nvptx~piclibs~strip languages=c,c++,fortran patches=ecc5ac43951b34cbc5db15f585b4e704c42e2e487f9ed4c24fadef3f3857930b arch=darwin-bigsur-skylake
    ^diffutils@2.8.1%apple-clang@12.0.5 arch=darwin-bigsur-skylake
    ^gmp@6.2.1%apple-clang@12.0.5 arch=darwin-bigsur-skylake
        ^autoconf@2.71%apple-clang@12.0.5 arch=darwin-bigsur-skylake
        ^automake@1.16.4%apple-clang@12.0.5 arch=darwin-bigsur-skylake
        ^libtool@2.4.6%apple-clang@12.0.5 arch=darwin-bigsur-skylake
        ^m4@1.4.6%apple-clang@12.0.5+sigsegv patches=c0a408fbffb7255fcc75e26bd8edab116fc81d216bfd18b473668b7739a4158e arch=darwin-bigsur-skylake
    ^libiconv@1.16%apple-clang@12.0.5 arch=darwin-bigsur-skylake
    ^mpc@1.1.0%apple-clang@12.0.5 arch=darwin-bigsur-skylake
        ^mpfr@4.1.0%apple-clang@12.0.5 arch=darwin-bigsur-skylake
            ^autoconf-archive@2019.01.06%apple-clang@12.0.5 arch=darwin-bigsur-skylake
            ^texinfo@4.8%apple-clang@12.0.5 arch=darwin-bigsur-skylake
    ^zlib@1.2.11%apple-clang@12.0.5+optimize+pic+shared arch=darwin-bigsur-skylake
    ^zstd@1.5.0%apple-clang@12.0.5~ipo~legacy~lz4~lzma~multithread+programs+shared+static~zlib build_type=RelWithDebInfo arch=darwin-bigsur-skylake
        ^cmake@3.21.1%apple-clang@12.0.5~doc+ncurses+openssl+ownlibs~qt build_type=Release arch=darwin-bigsur-skylake
```

如果想使用特定依赖或者依赖系统的包，会在 `~/.spack/package` 下得到，其使用方法可见 [AMD](https://developer.amd.com/spack/build-customization/)

```c
❯ spack external find

❯ cat ~/.spack/package.json
       │ File: /Users/victoryang/.spack/packages.yaml
───────┼────────────────────────────────────────────────────────────────────────────────────────────────────────────────
   1   │ packages:
   2   │   autoconf:
   3   │     externals:
   4   │     - spec: autoconf@2.71
   5   │       prefix: /usr/local
   6   │   automake:
   7   │     externals:
   8   │     - spec: automake@1.16.4
   9   │       prefix: /usr/local
  10   │   bash:
  11   │     externals:
  12   │     - spec: bash@3.2.57
  13   │       prefix: /
  14   │   bazel:
  15   │     externals:
  16   │     - spec: bazel@4.1.0
  17   │       prefix: /usr/local
  18   │   bison:
  19   │     externals:
  20   │     - spec: bison@2.3
  21   │       prefix: /usr
  22   │   bzip2:
  23   │     externals:
  24   │     - spec: bzip2@1.0.6
  25   │       prefix: /usr
  26   │   cmake:
  27   │     externals:
  28   │     - spec: cmake@3.21.1
  29   │       prefix: /usr/local
  30   │   diffutils:
  31   │     externals:
  32   │     - spec: diffutils@2.8.1
  33   │       prefix: /usr
...
```

安装后的参数大致需要的有几个，`-j N` 是 job 个数，`--no-checksum` 不检查文件 md5，`--no-restage` 为在修改过的文件后继续编译，一般在 `/tmp/root/spack-stage/spack-stage-amdscalapack-3.0-qwvyrumhsizxiaujwdsppcovijr5k5ri/spack-src/`. 有些包有 cflags cxxflags fcflags。 有些有 cuda_arch。碰到新的软件可以把所有需要的参数 append 到里面。

```c
❯ spack install -j 8 --no-checksum llvm+mlir+flang+all_targets+python+shared_libs cflags="-O3" cxxflags="-O3"
[+] /usr/local (external cmake-3.21.1-cdhzbrts4k5ylrvlpspfl75zgeht4swi)
[+] /Users/victoryang/spack/opt/spack/darwin-bigsur-skylake/apple-clang-12.0.5/libiconv-1.16-ropgshv657ooz7kfzojv4s6srscgimnw
[+] /usr/local (external pkg-config-0.29.2-4nv7fo7lbjybt2u3xzb2vxzvgvaz5xmw)
[+] /usr/local (external xz-5.2.5-p37wr6fna4ysoh2xn2wnmmzttm3bi37o)
[+] /Users/victoryang/spack/opt/spack/darwin-bigsur-skylake/apple-clang-12.0.5/zlib-1.2.11-lci2s4zd6x77rmexa3uuarbl5cvneskw
[+] /usr (external perl-5.30.2-4zkfgqml35km4ly7xmxn7ooz44dxtgqp)
[+] /usr/local (external python-3.9.6-shbb7dthsqe4lu26jugddyi2k7pl3jbl)
[+] /Users/victoryang/spack/opt/spack/darwin-bigsur-skylake/apple-clang-12.0.5/pcre-8.44-g4df4jqpudoxhjsrubrqhv3uwxajofet
[+] /usr/local (external z3-4.8.12-hvhfxnxuachtpi524zf55znqn55vanod)
[+] /Users/victoryang/spack/opt/spack/darwin-bigsur-skylake/apple-clang-12.0.5/ncurses-6.2-xilcz3bhw4otebvysduddyldezxhxvy6
[+] /Users/victoryang/spack/opt/spack/darwin-bigsur-skylake/apple-clang-12.0.5/libxml2-2.9.10-mlrnjcbnjt3w7635xrietes7terwhko6
[+] /Users/victoryang/spack/opt/spack/darwin-bigsur-skylake/apple-clang-12.0.5/perl-data-dumper-2.173-cv4kwshixb7tmk6p7icxrqpicppkx5gr
[+] /Users/victoryang/spack/opt/spack/darwin-bigsur-skylake/apple-clang-12.0.5/py-setuptools-50.3.2-hwyhyijgi3yjokddm67tb6aulefteudx
[+] /Users/victoryang/spack/opt/spack/darwin-bigsur-skylake/apple-clang-12.0.5/swig-4.0.2-vajpijk4isacr52dzgk2gqbvyunadwkc
[+] /Users/victoryang/spack/opt/spack/darwin-bigsur-skylake/apple-clang-12.0.5/libedit-3.1-20210216-6h4xokftdnxe2h3o7tie2cnbzbhfrr4h
[+] /Users/victoryang/spack/opt/spack/darwin-bigsur-skylake/apple-clang-12.0.5/hwloc-2.5.0-z2brjfcvnend5gorjmeqqgirccqerdwd
[+] /Users/victoryang/spack/opt/spack/darwin-bigsur-skylake/apple-clang-12.0.5/py-six-1.15.0-c63zkkdjpvegqai2f4jjg4mutsuchoov
[+] /Users/victoryang/spack/opt/spack/darwin-bigsur-skylake/apple-clang-12.0.5/llvm-12.0.1-n6c5z7sqfo7olnaqswu7jqhcdkyyk6nh
```

以依赖 nvhpc 编译器的 hdf5 为例。笔者碰到的问题有给定 mpi 找不到对应 wrapper 的 nvc 以及 nvfortran。在手动编译的时候会在 PATH 里面找 cc，或者直接找 CC，FC，CXX。这时候需要定义一个 FC。

```c
if spec.compiler.fc="nvfortran":
   env.set("FC","/path/to/mpifort wrapper")
   args.append(("CMAKE_Fortran_COMPILER",spec.complier.mpifort))
```

在超算上的 spack 有两个 upstream，你觉得重要可以直接给原 repo PR，一般我们备份到学校内部的 gitlab。

#### Spack 与 Modules
当系统有 Modules 时，会自动把 Module file 的目录加到 MANPATH 下，也即立即可以使用 `module load`