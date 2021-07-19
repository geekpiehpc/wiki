<!-- TITLE: CESM -->
<!-- SUBTITLE: CESM summary -->

# CESM

## Build & Running

### OneKeyConf

```bash
./create_newcase -res 0.47x0.63_gx1v6 -compset B -case ../EXP2 -mach pleiades-ivy
mkdir nobackup
ln -s /home/cesm/data/inputdata_EXP1/ nobackup/inputdata
# EXP1: ./xmlchange -file env_run.xml -id DOCN_SOM_FILENAME -val pop_frc.gx1v6.091112.nc
./xmlchange -file env_build.xml -id CESMSCRATCHROOT -val `pwd`'/nobackup/$USER'
./xmlchange -file env_build.xml -id EXEROOT -val `pwd`'/nobackup/$CCSMUSER/$CASE/bld'
./xmlchange -file env_run.xml -id RUNDIR -val `pwd`'/nobackup/$CCSMUSER/$CASE/run'
./xmlchange -file env_run.xml -id DIN_LOC_ROOT -val `pwd`'/nobackup/inputdata'
./xmlchange -file env_run.xml -id DIN_LOC_ROOT_CLMFORC -val `pwd`'/nobackup/inputdata/atm/datm7'
./xmlchange -file env_run.xml -id DOUT_S_ROOT -val `pwd`'/nobackup/$CCSMUSER/archive/$CASE'
./xmlchange -file env_run.xml -id RUN_STARTDATE -val 2000-01-01
./xmlchange -file env_build.xml -id BUILD_THREADED -val TRUE
# edit Macro SLIBS -lnetcdff
# edit env_mach_specific
./cesm_setup
```

### ybs.sh

```bash
./EXP2.clean_build all
./cesm_setup -clean
rm -rf $build_dir
./cesm_setup
./EXP2.build
```

### PBS

```
##PBS -N dappur
##PBS -q pub_blad_2
##PBS -j oe
##PBS -l walltime=00:01:00
##PBS -l nodes=1:ppn=28
```


## Performance Tuning

## Trouble Shooting

### High sys percentage in top (>20%)

This is apparent this is a communication problem. Try switching to Intel MPI for a terribly low sys percentage (\<1%).

### ERROR: remap transport: bad departure points

```
Warning: Departure points out of bounds in remap                  
 my_task, i, j =         182           4           8              
 dpx, dpy =  -5925130.21408796      -0.368922055964299            
 HTN(i,j), HTN(i+1,j) =   72848.1354852604        72848.1354852604
 HTE(i,j), HTE(i,j+1) =   59395.4550164223        59395.4550164223
 istep1, my_task, iblk =     1095001         182           1      
 Global block:         205                                        
 Global i and j:          35          47                          
(shr_sys_abort) ERROR: remap transport: bad departure points      
(shr_sys_abort) WARNING: calling shr_mpi_abort() and stopping     
application called MPI_Abort(MPI_COMM_WORLD, 1001) - process 182
```

This error may due to multiple reasons.

One significant one is the bad grid division. We were once using one PE for every processor core so the total number of PEs is not a power of 2. Then we used 128 (or later 256) and the error diminished until it showed up again after 6mos of simulation...

Then another affecting reason is the parameter xndt_dyn, see link. This parameter has already been set to 2 after solving the last problem (originally 1). Then we tried increasing this parameter again, it passed the 6mos simulation, but crashed again after another 3mos. We then continued increasing the value, but it crashes faster. We stopped at about 20mos simulation and turned to GNU compiler version with Intel MPI.

However, this does not mean it's the fault of Intel compiler. Direct comparison between Intel and GNU compilers is unfair because the combination of Intel compiler xndt_dyn=1 and most importantly the correct PE number has not been tried. Maybe try using xndt_dyn=1 from be beginning next time, using Intel compiler.

### OpenMP failed

Still no solved, but very promising for improving performance.


