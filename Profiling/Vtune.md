# Vtune
都说做体系结构的人最懂做 Profiling，一个好的 Profiling 工具一定是有好的对 CPU 实时性能的抽象，最简单的命令是 `rdstc`， arm上也有类似的实现

## ABI 支持
profiler 需要有对 Intel Proceccor 各种 metrics 的函数实现，在安装Vtune的过程中会编译对当前系统的 [PMU](https://software.intel.com/content/www/us/en/develop/articles/intel-performance-counter-monitor.html) 的动态链接库，意为对 Intel [PMU](https://software.intel.com/content/www/us/en/develop/articles/intel-performance-counter-monitor.html) 的 ABI支持，我队使用的 epyc 有epyc适用的魔改版 [PMU Tools]( https://github.com/AMDESE/amd-perf-tools)。现在体系结构安全界学术主流对 PMU 的研究很深，因为其泄露了部分对 CPU 实时的状态，可以从中获取想要的东西。

X86 需要支持的 perf 参数比较有限，linux从kprobe，uprobe这些官方支持的 microprocessor sampling有很有用的，这些已被eBPF所采用。

Intel Compiler 在编译 broadwell 以上架构优化时主要做了三件对性能影响很大的事情：
1. 激进的跨 basic block 优化 + Vectorization + Loop Unroll
2. Load 和 Store 在满足 TSO 条件下的激进的重排，同时激进的整合数据，支持 store buffer bypass，movnt。同时也是 icc 后端 Bug 的主要来源。也是大厂不太用他的原因。除 HPC 外，大家一般照 gcc 标准。
3. 自己维护的 TBB 线程池（非常快），自己维护的 malloc_align，自己维护的相关库。

有关如何更好的适配 Intel 的CPU，可以参考 Lammps 的 [Intel Package](https://t.co/6DUtP6Falq?amp=1)。其使用访问者模式对 Intel processor的寄存器资源。

有关对用户态文件系统的适配，Vtune 提供了对 PM 带宽的实时测试，这个 metrics 貌似很难拿到。

## 在集群上如何使用

```
spack load intel-parallel-studio # choose the right version
amplxe-cl 
```