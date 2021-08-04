# 机器简介
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