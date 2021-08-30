# 机器简介
## epyc 7742 introduction
```bash
Architecture:        x86_64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
Address sizes:       43 bits physical, 48 bits virtual
CPU(s):              256
On-line CPU(s) list: 0-255
Thread(s) per core:  2
Core(s) per socket:  64
Socket(s):           2
NUMA node(s):        2
Vendor ID:           AuthenticAMD
CPU family:          23
Model:               49
Model name:          AMD EPYC 7742 64-Core Processor
Stepping:            0
CPU MHz:             2997.123
CPU max MHz:         2250.0000
CPU min MHz:         1500.0000
BogoMIPS:            4499.77
Virtualization:      AMD-V
L1d cache:           32K
L1i cache:           32K
L2 cache:            512K
L3 cache:            16384K
NUMA node0 CPU(s):   0-63,128-191
NUMA node1 CPU(s):   64-127,192-255
Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx mmxext fxsr_opt pdpe1gb rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid extd_apicid aperfmperf pni pclmulqdq monitor ssse3 fma cx16 sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx f16c rdrand lahf_lm cmp_legacy svm extapic cr8_legacy abm sse4a misalignsse 3dnowprefetch osvw ibs skinit wdt tce topoext perfctr_core perfctr_nb bpext perfctr_llc mwaitx cpb cat_l3 cdp_l3 hw_pstate sme ssbd sev ibrs ibpb stibp vmmcall fsgsbase bmi1 avx2 smep bmi2 cqm rdt_a rdseed adx smap clflushopt clwb sha_ni xsaveopt xsavec xgetbv1 xsaves cqm_llc cqm_occup_llc cqm_mbm_total cqm_mbm_local clzero irperf xsaveerptr arat npt lbrv svm_lock nrip_save tsc_scale vmcb_clean flushbyasid decodeassists pausefilter pfthreshold avic v_vmsave_vmload vgif umip rdpid overflow_recov succor smca
```

$$
\begin{array}{|l|c|}
\hline \text { Cache level/type } & \text { Size \&Information } \\
\hline \text { L } 0 \mu \text { OP } & 4,096 \mu \text { OPs, 8-way set associative,64-sets, 8-$\mu$ OP line size } \\
\hline \text { L1 instruction } & \begin{array}{c}
64 \text { KiB 4-way set associative,256-sets, 64 B } \\
\text { line size, shared by the two threads, per core }
\end{array} \\
\hline \text { L1 data } & \begin{array}{c}
32 \mathrm{KiB} \text { 8-way set associative, 64-sets, 64 B line size, write- } \\
\text { back policy, 4-5 cycles latency for Int, 7-8 cycles latency for FP }
\end{array} \\
\hline \text { L2 } & \begin{array}{c}
512 \text { KiB 8-way set associative, 1,024-sets, 64 B line size, } \\
\text { write-back policy, Inclusive of } L 1,17 \text { cycles latency }
\end{array} \\
\hline \text { L3 } & \begin{array}{c}
\text { Victim cache, } 16 \text { MiB/CCX, shared across all cores, 16-way } \\
\text { set associative, } 16,384 \text {-sets, } 64 \text { B line size, } 40 \text { cycles latency }
\end{array} \\
\hline \text { TLB instructions } & \begin{array}{c}
8 \text { entry L0 TLB, all page sizes, } 64 \text { entry L1 TLB, } \\
\text { all page sizes, } 512 \text { entry L2 TLB, no 1G pages }
\end{array} \\
\hline \text { TLB data } & 64 \text { entry L1 TLB, all page sizes, 1,532-entry L2 TLB, no 1G pages } \\
\hline
\end{array}
$$

![](https://en.wikichip.org/w/images/thumb/f/f2/zen_2_core_diagram.svg/1800px-zen_2_core_diagram.svg.png)

AMD claim that theoretical floating point performance can be calculated as: Double Precision theoretical Floating Point performance $=$ #real_cores $* 8$ DP flop/clk $*$ core frequency. For a 2 socket system $=2 * 64$ cores $* 8 \mathrm{DP}$ flops/ clk $* 2.2 \mathrm{GHz}=2252.8$ Gflops. This includes counting FMA as two flops.

## GPU

```bash
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 465.19.01    Driver Version: 465.19.01    CUDA Version: 11.3     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA Tesla V1...  On   | 00000000:01:00.0 Off |                    0 |
| N/A   38C    P0    44W / 250W |  26730MiB / 32510MiB |    100%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   1  NVIDIA Tesla V1...  On   | 00000000:61:00.0 Off |                    0 |
| N/A   40C    P0    46W / 250W |  17711MiB / 32510MiB |     14%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   2  NVIDIA Tesla V1...  On   | 00000000:A1:00.0 Off |                    0 |
| N/A   47C    P0    53W / 250W |   7825MiB / 32510MiB |     74%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   3  NVIDIA Tesla V1...  On   | 00000000:C1:00.0 Off |                    0 |
| N/A   33C    P0    40W / 250W |   1255MiB / 32510MiB |    100%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|    0   N/A  N/A     47300      C   python                          25475MiB |
|    0   N/A  N/A    243387      C   ...ang2/anaconda3/bin/python     1251MiB |
|    1   N/A  N/A    102972      C   python                          17707MiB |
|    2   N/A  N/A    209376      C   python                           7821MiB |
|    3   N/A  N/A    243388      C   ...ang2/anaconda3/bin/python     1251MiB |
+-----------------------------------------------------------------------------+
```


```bash
CA 'mlx5_0'
	CA type: MT4119
	Number of ports: 1
	Firmware version: 16.29.2002
	Hardware version: 0
	Node GUID: 0xb8599f0300e66b78
	System image GUID: 0xb8599f0300e66b78
	Port 1:
		State: Active
		Physical state: LinkUp
		Rate: 100
		Base lid: 1
		LMC: 0
		SM lid: 6
		Capability mask: 0x2651e84a
		Port GUID: 0xb8599f0300e66b78
		Link layer: InfiniBand
```