<!-- TITLE: HPL hpl.dat Config -->
<!-- SUBTITLE: Ancestral hpl.dat config file -->

# HPL .dat config file
## ASC18
The following is the HPL `.dat` configuration file template from ASC18.

```
HPLinpack benchmark input file
Innovative Computing Laboratory, University of Tennessee
HPL.out      output file name (if any)
6            device out (6=stdout,7=stderr,file)
1            # of problems sizes (N)
67200 65280 62976 65280 96000 65280 38400 96000 102400 168960 153600 76800   142848 153600 142848 124416 96256 142848 124416 115200 110592 96256 Ns
1             # of NBs
384 768 384 768 1024 768 896 768 1024 512 384 640 768 896 960 1024 1152 1280 384 640 960 768 640 256  960 512 768 1152         NBs
0            PMAP process mapping (0=Row-,1=Column-major)
1            # of process grids (P x Q)
2 1 2 1        Ps
1 2 2 4        Qs
16.0         threshold
1            # of panel fact
0 1 2        PFACTs (0=left, 1=Crout, 2=Right)
1            # of recursive stopping criterium
2 8          NBMINs (>= 1)
1            # of panels in recursion
2            NDIVs
1            # of recursive panel fact.
0 1 2        RFACTs (0=left, 1=Crout, 2=Right)
1            # of broadcast
2 0 2          BCASTs (0=1rg,1=1rM,2=2rg,3=2rM,4=Lng,5=LnM)
1            # of lookahead depth
0            DEPTHs (>=0)
1            SWAP (0=bin-exch,1=long,2=mix)
192          swapping threshold
1            L1 in (0=transposed,1=no-transposed) form
0            U  in (0=transposed,1=no-transposed) form
1            Equilibration (0=no,1=yes)
8            memory alignment in double (> 0)
```

## ASC20
The following is the HPL `.dat` configuration file template from ASC20.
Machine Spec : 8 Tesla V100

```
HPLinpack benchmark input file
Innovative Computing Laboratory, University of Tennessee
HPL.out      output file name (if any)
6            device out (6=stdout,7=stderr,file)
2            # of problems sizes (N)
175104 178176 165888 168960 172032 175104 Ns
2             # of NBs
384 256 128 256 384 192 288 320 384 384 768 1024 768 896 768 1024 512 384 640 768 896 960 1024 1152 1280 384 640 960 768 640 256  960 512 768 1152         NBs
0            PMAP process mapping (0=Row-,1=Column-major)
1            # of process grids (P x Q)
4 2 8 1 2 1        Ps
4 8 2 2 4        Qs
16.0         threshold
1            # of panel fact
0 1 2        PFACTs (0=left, 1=Crout, 2=Right)
1            # of recursive stopping criterium
2 8          NBMINs (>= 1)
1            # of panels in recursion
2            NDIVs
1            # of recursive panel fact.
0 1 2        RFACTs (0=left, 1=Crout, 2=Right)
1            # of broadcast
2 0 2          BCASTs (0=1rg,1=1rM,2=2rg,3=2rM,4=Lng,5=LnM)
1            # of lookahead depth
0            DEPTHs (>=0)
1            SWAP (0=bin-exch,1=long,2=mix)
192          swapping threshold
1            L1 in (0=transposed,1=no-transposed) form
0            U  in (0=transposed,1=no-transposed) form
1            Equilibration (0=no,1=yes)
8            memory alignment in double (> 0)
```

