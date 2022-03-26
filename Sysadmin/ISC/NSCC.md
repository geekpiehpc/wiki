---
title: NSCC
description: 
published: true
date: 2022-03-21T03:12:48.600Z
tags: 
editor: markdown
dateCreated: 2022-03-21T02:23:12.832Z
---

# NSCC

Used in [ISC21](../../Apps/ISC/ISC-21/README.md)

Login is very old, which is E5-2690 with centos6. The file system is old Lustre without flock(), so you have to disable spack check. The schedule system is using OpenPBS>

```bash
$ cat outputfile.o
Checking The CPU and Network
lscpu
Architecture:          x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                24
On-line CPU(s) list:   0-23
Thread(s) per core:    1
Core(s) per socket:    12
Socket(s):             2
NUMA node(s):          4
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 63
Model name:            Intel(R) Xeon(R) CPU E5-2690 v3 @ 2.60GHz
Stepping:              2
CPU MHz:               1200.000
BogoMIPS:              5187.61
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              15360K
NUMA node0 CPU(s):     0-5
NUMA node1 CPU(s):     6-11
NUMA node2 CPU(s):     12-17
NUMA node3 CPU(s):     18-23

lspci | grep Mel
81:00.0 Infiniband controller: Mellanox Technologies MT27700 Family [ConnectX-4]
======================================================================================

        Resource Usage on 2020-04-25 10:08:30.888043:

 JobId: 9954616.wlm01
 Project: 21120227
 Exit Status: 0
 NCPUs Requested: 1         NCPUs Used: 1
                CPU Time Used: 00:00:00
 Memory Requested: 100mb               Memory Used: 0kb
                Vmem Used: 0kb
 Walltime requested: 00:10:00           Walltime Used: 00:00:00

 Execution Nodes Used: (std1708:ncpus=1:mem=102400kb)

 ======================================================================================
```
DGX node is good for its hack on TDP of V100-16GB.

https://help.nscc.sg/wp-content/uploads/AI_System_QuickStart.pdf

```bash
Architecture:        x86_64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
CPU(s):              80
On-line CPU(s) list: 0-79
Thread(s) per core:  2
Core(s) per socket:  20
Socket(s):           2
NUMA node(s):        2
Vendor ID:           GenuineIntel
CPU family:          6
Model:               79
Model name:          Intel(R) Xeon(R) CPU E5-2698 v4 @ 2.20GHz
Stepping:            1
CPU MHz:             2794.907
CPU max MHz:         3600.0000
CPU min MHz:         1200.0000
BogoMIPS:            4390.10
Virtualization:      VT-x
L1d cache:           32K
L1i cache:           32K
L2 cache:            256K
L3 cache:            51200K
NUMA node0 CPU(s):   0-19,40-59
NUMA node1 CPU(s):   20-39,60-79
Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr s
se sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc
cpuid aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid dca sse4_1 sse4
_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch cpuid_fault epb cat_l3 cdp
_l3 invpcid_single pti intel_ppin ssbd ibrs ibpb stibp tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust bmi1 hl
e avx2 smep bmi2 erms invpcid rtm cqm rdt_a rdseed adx smap intel_pt xsaveopt cqm_llc cqm_occup_llc cqm_mbm_total cqm_mb
m_local dtherm ida arat pln pts md_clear flush_l1d
              total        used        free      shared  buff/cache   available
Mem:            503          58         340           0         105         442
Swap:             0           0           0
OFED-internal-4.4-2.0.7:
Ubuntu 18.04.2 LTS \n \l

Linux dgx4105 4.15.0-54-generic #58-Ubuntu SMP Mon Jun 24 10:55:24 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
Filesystem                                        Size  Used Avail Use% Mounted on
udev                                              252G     0  252G   0% /dev
tmpfs                                              51G  3.2M   51G   1% /run
/dev/sda2                                         440G  395G   22G  95% /
tmpfs                                             252G   12K  252G   1% /dev/shm
tmpfs                                             5.0M     0  5.0M   0% /run/lock
tmpfs                                             252G     0  252G   0% /sys/fs/cgroup
/dev/sda1                                         487M  6.1M  481M   2% /boot/efi
/dev/sdb1                                         7.0T  4.9T  1.8T  74% /raid
192.168.160.101:/home                             3.4P  2.1P  1.4P  61% /home
[davidcho@nscc03 ~]$ cat !$
cat dgx4105.txt
Architecture:        x86_64
CPU op-mode(s):      32-bit, 64-bit
Byte Order:          Little Endian
CPU(s):              80
On-line CPU(s) list: 0-79
Thread(s) per core:  2
Core(s) per socket:  20
Socket(s):           2
NUMA node(s):        2
Vendor ID:           GenuineIntel
CPU family:          6
Model:               79
Model name:          Intel(R) Xeon(R) CPU E5-2698 v4 @ 2.20GHz
Stepping:            1
CPU MHz:             2794.907
CPU max MHz:         3600.0000
CPU min MHz:         1200.0000
BogoMIPS:            4390.10
Virtualization:      VT-x
L1d cache:           32K
L1i cache:           32K
L2 cache:            256K
L3 cache:            51200K
NUMA node0 CPU(s):   0-19,40-59
NUMA node1 CPU(s):   20-39,60-79
Flags:               fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant_tsc arch_perfmon pebs bts rep_good nopl xtopology nonstop_tsc cpuid aperfmperf pni pclmulqdq dtes64 monitor ds_cpl vmx smx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid dca sse4_1 sse4_2 x2apic movbe popcnt tsc_deadline_timer aes xsave avx f16c rdrand lahf_lm abm 3dnowprefetch cpuid_fault epb cat_l3 cdp_l3 invpcid_single pti intel_ppin ssbd ibrs ibpb stibp tpr_shadow vnmi flexpriority ept vpid fsgsbase tsc_adjust bmi1 hle avx2 smep bmi2 erms invpcid rtm cqm rdt_a rdseed adx smap intel_pt xsaveopt cqm_llc cqm_occup_llc cqm_mbm_total cqm_mbm_local dtherm ida arat pln pts md_clear flush_l1d
              total        used        free      shared  buff/cache   available
Mem:            503          58         340           0         105         442
Swap:             0           0           0
OFED-internal-4.4-2.0.7:
Ubuntu 18.04.2 LTS \n \l

Linux dgx4105 4.15.0-54-generic #58-Ubuntu SMP Mon Jun 24 10:55:24 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
Filesystem                                        Size  Used Avail Use% Mounted on
udev                                              252G     0  252G   0% /dev
tmpfs                                              51G  3.2M   51G   1% /run
/dev/sda2                                         440G  395G   22G  95% /
tmpfs                                             252G   12K  252G   1% /dev/shm
tmpfs                                             5.0M     0  5.0M   0% /run/lock
tmpfs                                             252G     0  252G   0% /sys/fs/cgroup
/dev/sda1                                         487M  6.1M  481M   2% /boot/efi
/dev/sdb1                                         7.0T  4.9T  1.8T  74% /raid
192.168.160.101:/home                             3.4P  2.1P  1.4P  61% /home
192.168.156.29@o2ib,192.168.156.30@o2ib:/scratch  2.8P  1.8P  993T  65% /scratch
tmpfs                                              51G     0   51G   0% /run/user/0
Sat May 23 06:15:29 2020
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 418.67       Driver Version: 418.67       CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  Tesla V100-SXM2...  On   | 00000000:0B:00.0 Off |                    0 |
| N/A   35C    P0    43W / 300W |      0MiB / 16130MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
06:00.0 3D controller: NVIDIA Corporation GV100GL [Tesla V100 SXM2 16GB] (rev a1)
07:00.0 3D controller: NVIDIA Corporation GV100GL [Tesla V100 SXM2 16GB] (rev a1)
0a:00.0 3D controller: NVIDIA Corporation GV100GL [Tesla V100 SXM2 16GB] (rev a1)
0b:00.0 3D controller: NVIDIA Corporation GV100GL [Tesla V100 SXM2 16GB] (rev a1)
85:00.0 3D controller: NVIDIA Corporation GV100GL [Tesla V100 SXM2 16GB] (rev a1)
86:00.0 3D controller: NVIDIA Corporation GV100GL [Tesla V100 SXM2 16GB] (rev a1)
89:00.0 3D controller: NVIDIA Corporation GV100GL [Tesla V100 SXM2 16GB] (rev a1)
8a:00.0 3D controller: NVIDIA Corporation GV100GL [Tesla V100 SXM2 16GB] (rev a1)
05:00.0 Infiniband controller: Mellanox Technologies MT27700 Family [ConnectX-4]
0c:00.0 Infiniband controller: Mellanox Technologies MT27700 Family [ConnectX-4]
84:00.0 Infiniband controller: Mellanox Technologies MT27700 Family [ConnectX-4]
8b:00.0 Infiniband controller: Mellanox Technologies MT27700 Family [ConnectX-4]
hca_id: mlx5_1
        transport:                      InfiniBand (0)
        fw_ver:                         12.23.1020
        node_guid:                      ec0d:9a03:00a4:bbde
        sys_image_guid:                 ec0d:9a03:00a4:bbde
        vendor_id:                      0x02c9
        vendor_part_id:                 4115
        hw_ver:                         0x0
        board_id:                       MT_2180110032
        phys_port_cnt:                  1
        Device ports:
                port:   1
                        state:                  PORT_ACTIVE (4)
                        max_mtu:                4096 (5)
                        active_mtu:             4096 (5)
                        sm_lid:                 251
                        port_lid:               1417
                        port_lmc:               0x00
                        link_layer:             InfiniBand

hca_id: mlx5_3
        transport:                      InfiniBand (0)
        fw_ver:                         12.23.1020
        node_guid:                      ec0d:9a03:00aa:2960
        sys_image_guid:                 ec0d:9a03:00aa:2960
        vendor_id:                      0x02c9
        vendor_part_id:                 4115
        hw_ver:                         0x0
        board_id:                       MT_2180110032
        phys_port_cnt:                  1
        Device ports:
                port:   1
                        state:                  PORT_ACTIVE (4)
                        max_mtu:                4096 (5)
                        active_mtu:             4096 (5)
                        sm_lid:                 251
                        port_lid:               1419
                        port_lmc:               0x00
                        link_layer:             InfiniBand

hca_id: mlx5_0
        transport:                      InfiniBand (0)
        fw_ver:                         12.23.1020
        node_guid:                      ec0d:9a03:00aa:29b8
        sys_image_guid:                 ec0d:9a03:00aa:29b8
        vendor_id:                      0x02c9
        vendor_part_id:                 4115
        hw_ver:                         0x0
        board_id:                       MT_2180110032
        phys_port_cnt:                  1
        Device ports:
                port:   1
                        state:                  PORT_ACTIVE (4)
                        max_mtu:                4096 (5)
                        active_mtu:             4096 (5)
                        sm_lid:                 251
                        port_lid:               1416
                        port_lmc:               0x00
                        link_layer:             InfiniBand

hca_id: mlx5_2
        transport:                      InfiniBand (0)
        fw_ver:                         12.23.1020
        node_guid:                      ec0d:9a03:00aa:2988
        sys_image_guid:                 ec0d:9a03:00aa:2988
        vendor_id:                      0x02c9
        vendor_part_id:                 4115
        hw_ver:                         0x0
        board_id:                       MT_2180110032
        phys_port_cnt:                  1
        Device ports:
                port:   1
                        state:                  PORT_ACTIVE (4)
                        max_mtu:                4096 (5)
                        active_mtu:             4096 (5)
                        sm_lid:                 251
                        port_lid:               1422
                        port_lmc:               0x00
                        link_layer:             InfiniBand
```

If NSCC is used again in the competition, contact your NTU or NUS student, they have four unique logins nodes to log in to the DGX nodes.

