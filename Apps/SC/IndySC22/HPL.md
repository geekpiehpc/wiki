---
title: HPL
description: HPL Benchmark
published: true
date: 2022-10-30T11:19:21.524Z
tags: benchmark, hpl
editor: markdown
dateCreated: 2022-10-30T10:57:07.530Z
---

# HPL & HPL Hero Run
此次 Indy SC 比赛中的 HPL benchmark 分为两组题目，一组为 HPL 在单节点上的调优，另外一组为在整个集群上运行 HPL (约300个节点).

## HPL

Hi teams

Here is the assignment on HPL!
Compute the theoretical peak FLOPs for the processor type available on Chameleon cloud for the IndySCC competition. (10 points)
Build and install the HPL benchmark using your choice of linear algebra library and MPI library. Which linear algebra library did you choose and why? Which MPI library did you choose and why? (20 points)
Run the HPL benchmark on a node using a fixed problem size (N) and by varying the number of cores from 2 to 20, doubling the cores at each trial. Which parameters did you need to change? Plot the GFLOPs number for each run vs. no. of cores. (30 points)
Run the HPL benchmark using all 20 cores and tune the HPL.dat file to achieve the best GFLOPs number. Which parameters did you tune? What were your results using the unoptimized parameter(s) vs. the optimized parameter(s)? (40 points)
(Bonus) Run HPL on 2-nodes. Was the GFLOPs number exactly twice that of your single-node performance? Why or why not? (10 points)
Deliverables:
Submit a report answering the questions in the assignment. While describing your steps, be brief and to the point. 
You should also include a description of your cluster and how you created the cluster.
For each of the steps 2-5, include the scripts that you used to build and run HPL. Provide sufficient documentation for your codes. Please note that your environment and scripts should be reproducible, i.e., the judges should be able to run them on an empty cluster. 
For the tuning step 4, include the final HPL.dat file that gave you the best performance.
Submission Instructions
The assignment is assigned to all students. However, a single submission per team is sufficient. One member of the team can submit the assignment.
The report can be a PDF file (preferred method) or a link to a google doc (we will check the timestamp for when it was last edited).
Please include your team name and the university in the report.

## HPL Hero Run

Between now and Oct 20 you are allowed to burst up to 30 compute nodes to test scaling your node deployment processes.
Hero HPL Runs
On Oct 20 we will begin hero HPL runs. Each team will get a 23 hour window where they will be allowed to use up 300 compute nodes to complete the best HPL score they can get. 
Ground Rules
During this time, the other teams will only be allowed to use their 1 head node to continue testing. At the beginning of this phase we will shut down any compute nodes still running. Resource usage during this time will be closely monitored - any interference, accidental or otherwise, with the competing team will be penalized per the IndySCC rules and at the discretion of the committee.
Schedule
Each window will begin at 9 am Eastern Time and will end at 8 am Eastern Time the following day. We will need an hour to make sure your nodes are shut down and ready to go for the next team. The schedule was randomly generated and assigned and is listed below. If you would like to swap dates, you are responsible for finding another team to swap with. You may use the Google classroom to ask if anyone else needs to swap and is willing. Once the first team begins, the schedule will be locked in place. Otherwise, we cannot change any dates unless mutually agreed upon and there isn’t enough time left in the month to have alternative days.

Thu Oct 20 - Durham University
Fri Oct 21 - CUHK
Sat Oct 22 - Georgia Tech
Sun Oct 23 - SUSTech
Mon Oct 24 - ShanghaiTech
Tue Oct 25 - CSC/Finland
Wed Oct 26 - Monash University
Thu Oct 27 - Clemson
Fri Oct 28 - U Indonesia
Sat Oct 29 - UTEP
Sun Oct 30 - TAMUCC
 Helpful Hints
A successful team may strategize to run HPL in increasing increments of nodes, rather than try for the full 300 nodes at once. It is OK if you don’t use the full number of nodes with your final score, getting hundreds of nodes to work nicely together in real-life benchmarking exercises takes time and may be difficult to do in the short period you have them. It would be advantageous to have a very good score on a smaller set of nodes than to struggle to get the full 300 running, run out of time, and not have a score at all.

Also keep in mind that this hardware is fairly old and there are likely a handful of bad/slow nodes. This is where slowly increasing the number of nodes you are using can come in handy.
Run Into Hardware Problems or Outages?

If you identify a slow node, we will not be able to fix it as the only spare parts are in the form of the other nodes, and it’s unlikely we will be able to fix it by the end of your window. You should exclude the slow node and move onto another node. If you scale up to the full 300 nodes, you may deploy extras, just make note of the slow nodes and document that your final submission uses no more than 300. 

If there is any disruption of resource availability during your window, such as a Chameleon outage or power/networking outage at the Purdue site, you may receive some make-up time in certain situations, as described in the next paragraph.

If the resources are cumulatively unavailable for more than 4 hours, at the discretion of the committee, you may receive an equivalent time slot (ie, if you were unable to access for 5 hours, you would get another 5 hours) plus an extra hour for node spin-up at the end to try again. This time would be disjoint and come at the end of the above schedule as we can’t shift the rest of the schedule for the remaining teams. 

If you are the last team, we will ask that you shut down at your designated time, as we need time to determine the outage length adjustments for everyone. You’ll be able to restart at a later time, and that will keep things fair as the other teams wouldn’t have the ability to just extend their window.

Outages of less than 4 hours will unfortunately be considered lost time. These shorter blips are part of real life challenges, and it would not be practical to allow make-up time for shorter times as you’d spend more time just spinning nodes back up.

We also cannot accommodate internet outages at your locale as we can’t verify those outages, so you may want to have a plan to find another connection if that happens.

Once all teams are done running, we will open the nodes back up for you to configure for the final 48 hour competition.
Submitting results

Final score submissions are due 1 hour after your window ends, right as the next team is starting. We will follow up with more details on this.

## 报告
[单节点](/answer.pdf)
[多节点](/hero_run_report.pdf)
[hero run 相关文件](/hero_run.tar)

## 参考资料
[Official Documentation](https://netlib.org/benchmark/hpl/documentation.html)
[AMD HPL Benchmark](https://developer.amd.com/spack/hpl-benchmark/)
[Run HPL on Threadripper](https://www.pugetsystems.com/labs/hpc/How-to-Run-an-Optimized-HPL-Linpack-Benchmark-on-AMD-Ryzen-Threadripper----2990WX-32-core-Performance-1291/)
[基于 HPL测试的集群系统性能分析与优化](https://www.jsjkx.com/CN/article/openArticlePDF.jsp?id=10370)

## 方案

### 调优 HPL.dat
具体过程请参见报告。
> 在 HPL.dat 中可以使用多组参数，这样跑一次 HPL 可以得到多个测试结果，效率会高一些。
{.is-info}


### 调优 MPI 参数

#### 进程还是线程
HPL 底层数学库可以利用多线程，所以可以让1个 HPL 进程利用整个 Socket。具体哪种的性能更好需要测试。

Intel 数学库有三套方案：单线程执行，OpenMP 和 Intel TBB，后两者可以利用多核。

具体调优情况参见报告。

### 绑核
numactl 可以用来绑定 HPL 使用的内存在哪一个 numa 上。
绑核的教程参见：
[IBM MPI documentation](https://www.ibm.com/docs/SSZTET_10.2/admin/smpi02_proc_affinity_mapbyunit.html)
[Understanding-MPI-map-by-and-bind-to-option](https://nekodaemon.com/2021/02/05/Understanding-MPI-map-by-and-bind-to-option/)