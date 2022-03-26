## AutoTuning 就是一个简单OI题
这题目本来是 NV 内部做 OSU 测试的一个小工具，拿出来给我们做题。题目要求根据不同 rank 之间的数据交换能力，做简单的调优，

### Task 1-3: Understand MPI_alltoallv calls 

Write a program with an input flag for pattern, on the Niagara cluster using 4 nodes, each with 40 ppn (full), total of 160 ppn, with balanced and unbalanced pattern.

The program should run 1000 iterations of MPI_alltoallv using the following characteristics.
### Task 4: Use Go+Front end to visualize the alltoallv pattern

We chose to use Go message passing because of no need for the performance dynamically and draw using `antd` design graph rather than draw directly using gnuplot2 which is legacy for display. The downsides is the frontend occupy too much memory and takes a little bit longer especially for the wrf.

### Task 5: Write a online algorithm to reaffine the pattern to makes it faster.

static calculation using MPI static analysis, and do a DP swap for data red heatmap part. [The code](https://github.com/geekpiehpc/collective_profiler)