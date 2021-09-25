# Cardioid

Repo: <https://github.com/LLNL/cardioid>

## 编译

> 如果使用 mfem，可能需要手动指定其路径。

### 自动编译

由于上游的 cardioid 安装包存在编译时会卡死的问题，因此需要手动修补安装文件。

首先运行 `spack edit cardioid` 命令，spack 将会启动文本编辑器。此后，在类 `class Cardioid(CMakePackage)` 的起始处加入以下内容

```
patch('https://gist.githubusercontent.com/KiruyaMomochi/cc4dfde7da51c3b11e45ab1079662693/raw/cardioid-cmake.patch',
    sha256='27e2b01a2a181d7364cf786f9da31193407b1aa9c20d0175965a3c772cc7378b')
```

此后使用 `spack -d install -v cardioid` 继续编译。

### Spack 手动编译

以 fish shell 为例。

```fish
source /opt/spack/share/spack/setup-env.fish
spack stage cardioid+cuda
spack cd cardioid+cuda
spack build-env cardioid+cuda fish
```

### 纯手动编译

TODO

## 问题解决

### Seg Fault with jemalloc

Happens when -nd >= 4

### SIGTERM after finishing the job with -np >= 60

Some issue in the openmpi@4.1.1/jip
​
#### Use the Intel MPI
​
```
spack load intel-oneapi-compilers@2021.1.2
​
export F90=ifort
export F77=ifort
export FC=ifort
export CC=icc
​
export LD_LIBRARY_PATH=/opt/spack/opt/spack/linux-debian10-zen2/gcc-10.2.0/intel-oneapi-compilers-2021.1.2-7ah54yk3newzc6hdcs3glm63clwyzgs7/mkl/2021.2.0/lib/intel64:$LD_LIBRARY_PATH
​
export LD_LIBRARY_PATH=/opt/spack/opt/spack/linux-debian10-zen2/gcc-10.2.0/intel-oneapi-compilers-2021.1.2-7ah54yk3newzc6hdcs3glm63clwyzgs7/mpi/2021.2.0/lib:$LD_LIBRARY_PATH
​
export LIBRARY_PATH=/opt/spack/opt/spack/linux-debian10-zen2/gcc-10.2.0/intel-oneapi-compilers-2021.1.2-7ah54yk3newzc6hdcs3glm63clwyzgs7/mpi/2021.2.0/lib:$LIBRARY_PATH
​
export LD_LIBRARY_PATH=/opt/spack/opt/spack/linux-debian10-zen2/gcc-10.2.0/intel-oneapi-compilers-2021.1.2-7ah54yk3newzc6hdcs3glm63clwyzgs7/mpi/2021.2.0/lib/release_mt:$LD_LIBRARY_PATH
​
export LIBRARY_PATH=/opt/spack/opt/spack/linux-debian10-zen2/gcc-10.2.0/intel-oneapi-compilers-2021.1.2-7ah54yk3newzc6hdcs3glm63clwyzgs7/mpi/2021.2.0/lib/release_mt:$LIBRARY_PATH
​
export PATH=/opt/spack/opt/spack/linux-debian10-zen2/gcc-10.2.0/intel-oneapi-compilers-2021.1.2-7ah54yk3newzc6hdcs3glm63clwyzgs7/mpi/2021.2.0/bin:$PATH
​
export MPI_LIBS=-L/opt/spack/opt/spack/linux-debian10-zen2/gcc-10.2.0/intel-oneapi-compilers-2021.1.2-7ah54yk3newzc6hdcs3glm63clwyzgs7/mpi/2021.2.0/libc
​
./configure --enable-parallel --with-scalapack=yes --enable-openmp CFLAGS="-march=core-avx2 -fma -ftz -fomit-frame-pointer -g" FFLAGS="-O3 -march=core-avx2 -align array64byte -fma -ftz -fomit-frame-pointer -g" SCALAPACK_LIBS="-lmkl_scalapack_lp64 -lmkl_blacs_intelmpi_lp64 -mkl=parallel -lifcore" IFLAGS="-I/opt/spack/opt/spack/linux-debian10-zen2/gcc-10.2.0/intel-oneapi-compilers-2021.1.2-7ah54yk3newzc6hdcs3glm63clwyzgs7/mkl/2021.2.0/include -I/home/qe/q-e/include -I/opt/spack/opt/spack/linux-debian10-zen2/gcc-10.2.0/intel-oneapi-compilers-2021.1.2-7ah54yk3newzc6hdcs3glm63clwyzgs7/mpi/2021.2.0/include" 
```
​
This version is slower than before.
​
Observations about the testcase AUSURF112: Cannot utilize hyperthreading efficiently, with 128 processes bind to core with OMP=1, it is faster than any combinations of number of processes and OMP_NUM_THREADS that utilizes all the hyperthreads.
​
### About fftw

When we specify FFT_LIBS to configure of quantum-espresso 6.6, fft related macro are not defined. If FFTW_INCLUDE is defined, __FFTW is defined. Changing to amdfftw does not influence the running time.
​
```
export FFT_LIBS=-L/opt/spack/opt/spack/linux-debian10-zen2/gcc-10.2.0/amdfftw-3.0-di7xmgpsu564qqvfhajkazsnk5kknxwd/lib
​
export FFTW_INCLUDE=/opt/spack/opt/spack/linux-debian10-zen2/gcc-10.2.0/amdfftw-3.0-di7xmgpsu564qqvfhajkazsnk5kknxwd/include
```

### 调试 CMake 项目

To debug a CMake project:

1. Find cmake command from spack's log
2. Create a new directory to save build files
3. `cd` to the path and run cmake command
4. append `--trace-source=[path to CMakeLists.txt]` to the cmake command

Use `message` command to print a middle result. For more information see [CMake doc](https://cmake.org/cmake/help/latest/command/message.html).

### Spack install 安装时间太久

使用 `spack -d install -v [package name]` 输出调试日志。 

如果问题出现在 cmake 期间，可能是函数 `interface_link_libraries` 的问题。
该函数会递归的去寻找各个子项目的 include path，这时候相同的依赖会被多次 include。又因为 spack 环境中的 include path 很长，会生成超极长的include path（指数级），导致 cmake 卡死。

#### 解决方式 1

在他递归生成时加入检查

```cmake
foreach(lib ${libs})
    list(FIND searched ${lib} lib_has_been_searched)    
    #message(SEND_ERROR "+++ ${lib} ${lib_has_been_searched}")
    if (lib_has_been_searched EQUAL -1)
      get_recursive_list(recursive_val ${lib} ${prop} ${searched})
      foreach(val ${retval})
        if(NOT recursive_val)
          list(APPEND val ${recursive_val})
        else()
          if (val IN_LIST recursive_val)
            #message("Duplicate val!")
          else()
            list(APPEND val ${recursive_val})
          endif()
        endif()
      endforeach()
    endif()
endforeach()
```

#### 解决方式 2

使用以下补丁，在递归后删除重复项。

```patch
diff --git a/elec/CMakeLists.txt b/elec/CMakeLists.txt
index 4a526cb..ca92d2d 100644
--- a/elec/CMakeLists.txt
+++ b/elec/CMakeLists.txt
@@ -271,7 +271,7 @@ function(get_recursive_list retvar target prop)
   list(APPEND searched ${target})
   #message(SEND_ERROR "=== ${target} ${prop} ${searched}")

-  set(${retval} "")
+  set(retval "")
   get_property(propval TARGET ${target} PROPERTY ${prop} SET)
   if (propval)
     get_target_property(propval ${target} ${prop})
@@ -288,6 +288,10 @@ function(get_recursive_list retvar target prop)
     endif()
   endforeach()

+  if(NOT retval)
+    list(REMOVE_DUPLICATES retval)
+  endif()
+
   set(${retvar} ${retval} PARENT_SCOPE)
   #message(SEND_ERROR "--- ${target} ${prop} ${retval}")
 endfunction()
```

### 无法连接上国际互联网

Set http proxy to `192.168.100.5:1082`, or use

```sh
proxychains -q [command]
```

#### Config proxy for git

Set proxy

```sh
git config --global http.proxy http://192.168.100.5:1082
```

Unset proxy

```sh
git config --global --unset http.proxy
```
