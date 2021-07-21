# ramBLe



## Install

### Lib

[Boost](/Libs/Boost)

just add the code into `SConstruct` to tell `scons` where is the boost lib is.

```
libPaths.append("/opt/spack/opt/spack/linux-debian10-zen2/gcc-10.2.0/boost-1.70.0-m4ttgcfqixwe22z5kz7bpp7mbqdspdbg/lib")
cppPaths.append("/opt/spack/opt/spack/linux-debian10-zen2/gcc-10.2.0/boost-1.70.0-m4ttgcfqixwe22z5kz7bpp7mbqdspdbg/include")
```