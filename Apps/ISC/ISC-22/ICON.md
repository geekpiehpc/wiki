---
title: ICON
description: 
published: true
date: 2022-04-23T08:06:22.718Z
tags: 
editor: markdown
dateCreated: 2022-03-21T02:22:59.229Z
---

# ICON

## Prepare

```bash
git clone https://gitlab.dkrz.de/icon-scc-isc22/icon-scc
cd /path/to/icon-scc
git submodule init 
git submodule update
```
## How to run
### spack compile
```bash
spack install -j (nproc) -vvvv icon%gcc@6.4.0
```
There are some varients:
- debug
- cuda
- openmp

### run copy scripts
```bash
cd {ICON_BUILD_DIR}
export ICON_DIR={ICON_DIR}
# Copy runscript-related files when building out-of-source:
if test $(pwd) != $(cd "${ICON_DIR}"; pwd); then
  echo "Copying runscript input files from the source directory..."
  rsync -uavz ${ICON_DIR}/run . --exclude='*.in' --exclude='.*' --exclude='standard_*'
  ln -sf -t run/ ${ICON_DIR}/run/standard_*
  ln -sf set-up.info run/SETUP.config
  rsync -uavz ${ICON_DIR}/externals . --exclude='.git' --exclude='*.f90' --exclude='*.F90' --exclude='*.c' --exclude='*.h' --exclude='*.Po' --exclude='tests' --exclude='rrtmgp*.nc' --exclude='*.mod' --exclude='*.o'
  rsync -uavz ${ICON_DIR}/make_runscripts .
  ln -sf ${ICON_DIR}/data
  ln -sf ${ICON_DIR}/vertical_coord_tables
fi
```
### Gen sbatch 
```bash
cd {ICON_BUILD_DIR}
export ICON_DIR={ICON_DIR}
cd {ICON_BUILD_DIR}/run
$ICON_DIR/utils/mkexp/mkexp standard_experiments/scc.config CO2=2850
```
OK if 
```bash
Script directory: '/mnt/nfs4/node1/home/qinfr/spack/opt/spack/linux-ubuntu20.04-zen/gcc-6.4.0/icon-2021-isc-scc-hw7pyldsuxsug2jrnmhdulvk5knzbzw6/experiments/exp_scc2850/scripts'
Data directory: '/mnt/nfs4/node1/home/qinfr/spack/opt/spack/linux-ubuntu20.04-zen/gcc-6.4.0/icon-2021-isc-scc-hw7pyldsuxsug2jrnmhdulvk5knzbzw6/experiments/exp_scc2850/outdata'
Work directory: '/mnt/nfs4/node1/home/qinfr/spack/opt/spack/linux-ubuntu20.04-zen/gcc-6.4.0/icon-2021-isc-scc-hw7pyldsuxsug2jrnmhdulvk5knzbzw6/experiments/exp_scc2850/work'
```
### Modify sbatch
In `experiments/exp_scc2850/scripts/exp_scc2850.run_start`
- FIX SLURM args
- FIX path
	- no `/build/`
  - no `/home/qinfr`
```git
- BUILD_DIR=/home/qinfr/spack/opt/spack/linux-ubuntu20.04-zen/gcc-6.4.0/icon-2021-isc-scc-hw7pyldsuxsug2jrnmhdulvk5knzbzw6/BUILD
+ BUILD_DIR=/mnt/nfs4/node1/home/qinfr/spack/opt/spack/linux-ubuntu20.04-zen/gcc-6.4.0/icon-2021-isc-scc-hw7pyldsuxsug2jrnmhdulvk5knzbzw6/
+ export PATH={cdo-1.9.10_BUILD_DIR}/bin:$PATH
...
```


  
- Subsitute all `/home/qinfr` to `/mnt/nfs4/node1/home/qinfr/`
### Run
```bash
sbatch exp_scc2850.run_start
```
## Tips
### How to check if compiled code uses SSE and AVX instructions?
[https://stackoverflow.com/questions/47878352/how-to-check-if-compiled-code-uses-sse-and-avx-instructions](https://stackoverflow.com/questions/47878352/how-to-check-if-compiled-code-uses-sse-and-avx-instructions)

```bash
objdump -d cgribexlib.o | awk '/[ \t](vmovapd|vmulpd|vaddpd|vsubpd|vfmadd213pd|vfmadd231pd|vfmadd132pd|vmu
lsd|vaddsd|vmosd|vsubsd|vbroadcastss|vbroadcastsd|vblendpd|vshufpd|vroundpd|vroundsd|vxorpd|vfnmadd231pd|vfnmadd213pd|vf
nmadd132pd|vandpd|vmaxpd|vmovmskpd|vcmppd|vpaddd|vbroadcastf128|vinsertf128|vextractf128|vfmsub231pd|vfmsub132pd|vfmsub2
13pd|vmaskmovps|vmaskmovpd|vpermilps|vpermilpd|vperm2f128|vzeroall|vzeroupper|vpbroadcastb|vpbroadcastw|vpbroadcastd|vpb
roadcastq|vbroadcasti128|vinserti128|vextracti128|vpminud|vpmuludq|vgatherdpd|vgatherqpd|vgatherdps|vgatherqps|vpgatherd
d|vpgatherdq|vpgatherqd|vpgatherqq|vpmaskmovd|vpmaskmovq|vpermps|vpermd|vpermpd|vpermq|vperm2i128|vpblendd|vpsllvd|vpsll
vq|vpsrlvd|vpsrlvq|vpsravd|vblendmpd|vblendmps|vpblendmd|vpblendmq|vpblendmb|vpblendmw|vpcmpd|vpcmpud|vpcmpq|vpcmpuq|vpc
mpb|vpcmpub|vpcmpw|vpcmpuw|vptestmd|vptestmq|vptestnmd|vptestnmq|vptestmb|vptestmw|vptestnmb|vptestnmw|vcompresspd|vcomp
ressps|vpcompressd|vpcompressq|vexpandpd|vexpandps|vpexpandd|vpexpandq|vpermb|vpermw|vpermt2b|vpermt2w|vpermi2pd|vpermi2
ps|vpermi2d|vpermi2q|vpermi2b|vpermi2w|vpermt2ps|vpermt2pd|vpermt2d|vpermt2q|vshuff32x4|vshuff64x2|vshuffi32x4|vshuffi64
x2|vpmultishiftqb|vpternlogd|vpternlogq|vpmovqd|vpmovsqd|vpmovusqd|vpmovqw|vpmovsqw|vpmovusqw|vpmovqb|vpmovsqb|vpmovusqb
|vpmovdw|vpmovsdw|vpmovusdw|vpmovdb|vpmovsdb|vpmovusdb|vpmovwb|vpmovswb|vpmovuswb|vcvtps2udq|vcvtpd2udq|vcvttps2udq|vcvt
tpd2udq|vcvtss2usi|vcvtsd2usi|vcvttss2usi|vcvttsd2usi|vcvtps2qq|vcvtpd2qq|vcvtps2uqq|vcvtpd2uqq|vcvttps2qq|vcvttpd2qq|vc
vttps2uqq|vcvttpd2uqq|vcvtudq2ps|vcvtudq2pd|vcvtusi2ps|vcvtusi2pd|vcvtusi2sd|vcvtusi2ss|vcvtuqq2ps|vcvtuqq2pd|vcvtqq2pd|
vcvtqq2ps|vgetexppd|vgetexpps|vgetexpsd|vgetexpss|vgetmantpd|vgetmantps|vgetmantsd|vgetmantss|vfixupimmpd|vfixupimmps|vf
ixupimmsd|vfixupimmss|vrcp14pd|vrcp14ps|vrcp14sd|vrcp14ss|vrndscaleps|vrndscalepd|vrndscaless|vrndscalesd|vrsqrt14pd|vrs
qrt14ps|vrsqrt14sd|vrsqrt14ss|vscalefps|vscalefpd|vscalefss|vscalefsd|valignd|valignq|vdbpsadbw|vpabsq|vpmaxsq|vpmaxuq|v
pminsq|vpminuq|vprold|vprolvd|vprolq|vprolvq|vprord|vprorvd|vprorq|vprorvq|vpscatterdd|vpscatterdq|vpscatterqd|vpscatter
qq|vscatterdps|vscatterdpd|vscatterqps|vscatterqpd|vpconflictd|vpconflictq|vplzcntd|vplzcntq|vpbroadcastmb2q|vpbroadcast
mw2d|vexp2pd|vexp2ps|vrcp28pd|vrcp28ps|vrcp28sd|vrcp28ss|vrsqrt28pd|vrsqrt28ps|vrsqrt28sd|vrsqrt28ss|vgatherpf0dps|vgath
erpf0qps|vgatherpf0dpd|vgatherpf0qpd|vgatherpf1dps|vgatherpf1qps|vgatherpf1dpd|vgatherpf1qpd|vscatterpf0dps|vscatterpf0q
ps|vscatterpf0dpd|vscatterpf0qpd|vscatterpf1dps|vscatterpf1qps|vscatterpf1dpd|vscatterpf1qpd|vfpclassps|vfpclasspd|vfpcl
assss|vfpclasssd|vrangeps|vrangepd|vrangess|vrangesd|vreduceps|vreducepd|vreducess|vreducesd|vpmovm2d|vpmovm2q|vpmovm2b|
vpmovm2w|vpmovd2m|vpmovq2m|vpmovb2m|vpmovw2m|vpmullq|vpmadd52luq|vpmadd52huq|v4fmaddps|v4fmaddss|v4fnmaddps|v4fnmaddss|v
p4dpwssd|vp4dpwssds|vpdpbusd|vpdpbusds|vpdpwssd|vpdpwssds|vpcompressb|vpcompressw|vpexpandb|vpexpandw|vpshld|vpshldv|vps
hrd|vpshrdv|vpopcntd|vpopcntq|vpopcntb|vpopcntw|vpshufbitqmb|gf2p8affineinvqb|gf2p8affineqb|gf2p8mulb|vpclmulqdq|vaesdec
|vaesdeclast|vaesenc|vaesenclast)[ \t]/'
```