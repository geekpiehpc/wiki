---
title: uProf
description: 
published: true
date: 2022-03-27T18:10:28.575Z
tags: 
editor: markdown
dateCreated: 2022-03-21T02:21:11.878Z
---

# uProf
AMD 出品的一款 perf 工具，增加了一些 X86 超集的 metrics，但 UI 比较丑。

## x86 subset
You can refer to the `CONFIG_X86_AMD_PSTATE` tuning on [ZenStates-Linux](https://github.com/geekpiehpc/ZenStates-Linux). The perf subset could be found in the linux perf tools. Also check `/sys/devices/system/cpu/cpu*/cpufreq/scaling_driver` to see if the governor is available. The feature is switched on from Linux 5.17 (Ubuntu 22.04).

## Reference
1. https://faculty.sites.uci.edu/zhouli/files/2022/01/oakland22.pdf
2. https://indico.cern.ch/event/730908/contributions/3153163/attachments/1730954/2810149/epyc.pdf
3. https://www.nextplatform.com/2019/08/15/a-deep-dive-into-amds-rome-epyc-architecture/
4. https://github.com/FPSG-UIUC/lotr