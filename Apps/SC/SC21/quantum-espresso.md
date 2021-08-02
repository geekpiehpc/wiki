# Quantum Espresso

https://github.com/QEF/q-e

### compile

Could not find MPI (Missing MPI_FORTRAN_FOUND)

solve:  `-DMPIEXEC_EXECTUABLE=${MPI_HOME}/bin/mpiexec`

The compiled version does not support OpenMP and only support up to 4 processes for MPI.

Add the options: 

`-DQE_ENABLE_OPENMP=ON`
`-DCMAKE_Fortran_COMPILER=${MPI_HOME}/bin/mpifort`
`-DOpenMP_C_FLAGS=-fopenmp=lomp`
`-DOpenMP_CXX_FLAGS=-fopenmp=lomp`
`-DOpenMP_C_LIB_NAMES=libomp`
`-DOpenMP_CXXLIB_NAMES=libomp`
`-DOpenMP_libomp_LIBRARY=/usr/lib/x86_64-linux-gnu/libomp.so.5`

Change Toolchain to `System`.

Add `-g` to `CMakeList.txt` to get additional debug information.

`set(CMAKE_CXX_FLAGS -g)`
`set(CMAKE_C_FLAGS -g)`
`set(CMAKE_Fortran_FLAGS -g)`

https://www.quantum-espresso.org/Doc/user_guide/

library configure: https://www.quantum-espresso.org/Doc/user_guide/node11.html

### test

In directory `/q-e/test-suite/`, use `make run-tests` to test the correctness of basic functionalities. 

### run

`spack load ucx/gji`

`/home/qe/q-e/bin/pw.x`

To control the number of processors in each group, command line switches: -nimage, -npools, -nband, -ntg, -ndiag or -northo (shorthands, respectively: -ni, -nk, -nb, -nt, -nd) are used. As an example consider the following command line:
`mpirun -np 4096 ./neb.x -ni 8 -nk 2 -nt 4 -nd 144 -i my.input` This executes a NEB calculation on 4096 processors, 8 images (points in the configuration space in this case) at the same time, each of which is distributed across 512 processors. k-points are distributed across 2 pools of 256 processors each, 3D FFT is performed using 4 task groups (64 processors each, so the 3D real-space grid is cut into 64 slices), and the diagonalization of the subspace Hamiltonian is distributed to a square grid of 144 processors (12x12).

`mpirun -np 24 -x PATH --oversubscribe -x OMP_NUM_THREADS=4 -x LD_LIBRARY_PATH=/opt/nonspack/ucx-1.10.0-gcc/lib --allow-run-as-root /home/qe/q-e/bin/pw.x < ./ausurf.in`

First run with 24 processes and 4 thread each: 

![img](./quantum-espresso.png)

Problem: OMP threads can only use up to 200% CPU per process even with 256 threads per process. 

### Analyze

Using lizard

Fortran: 

    Total nloc  Avg.NLOC  AvgCCN  Avg.token  Fun Cnt  Warning cnt  Fun Rt  nloc Rt
    599949      54.1      10.6    569.7      9939     1693         0.17    0.58

C:

    Total nloc  Avg.NLOC  AvgCCN  Avg.token  Fun Cnt  Warning cnt  Fun Rt  nloc Rt
    52039       152.5     3.0     1050.3     323      19           0.06    0.53

Python:

    Total nloc  Avg.NLOC  AvgCCN  Avg.token  Fun Cnt  Warning cnt  Fun Rt  nloc Rt
    8864        18.3      5.0     146.0      298      21           0.07    0.26

