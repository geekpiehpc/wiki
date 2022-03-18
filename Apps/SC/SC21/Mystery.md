<link href="./Mystery.css" rel="stylesheet">
</link> 
<script src="./Mystery.js">
</script>

## Rules

<div id="my-crossword"></div>

## Install

```Bash
# Load gcc and openmpi:
module load gcc-9.2.0 module load mpi
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh bash Miniconda3-latest-Linux-x86_64.sh
# Accept license agreements and select the
# right install location, probably not your
# home directory since disk quota is limited. #
# Activate conda, if you skipped it's auto initialization source /path/to/your/conda/install/bin/activate
# Turn on the conda base environment
conda activate
# Install pytorch
conda install pytorch cudatoolkit=11.3 -c pytorch
# Install build dependencies for larcv3:
conda install cmake hdf5 scikit-build
# Install Tensorflow:
pip install tensorflow
# NOTE: if you don't install tensorflow, you need to pip install numpy!
# Clone larcv and install it:
git clone https://github.com/DeepLearnPhysics/larcv3.git cd larcv3
git submodule update --init
python setup.py build -j 64
python setup.py install
# Install mpi4py:
pip install --force-reinstall mpi4py --no-cache-dir
# Install horovod with tensorflow or if you want it with pytorch:
pip install --force-reinstall horovod --no-cache-dir
```

## 參數

```Bash
mode.optimizer.gradient_accumulation <= 1
mode.optimizer.learning_rate=123.456
mode.optimizer.name = "rmsprop" "adam"
mode.weights_location -> load checkpoint
mode.no_summary_images

run.compute_mode = DPCPP #？ data parallel Cpp intel MKL優化CPU

gradient_accumulation.....: 1
conf['mode']['optimizer']['learning_rate'] = 10.**random.uniform(-3.5, -2.5)
conf['mode']['optimizer']['loss_balance_scheme'] = random.choice(["none", "light", "focal"])
checkpoint_iteration........: 500

```

```YAML
learning_rateloss_balance_scheme
```

## SCC_21.yml

```YAML
defaults:
  - _self_
  - network: SCC_21
  - framework: torch
  - mode: train
  - data: real
data:
  downsample: 0
run:
  distributed: true
  iterations: 500
  compute_mode: GPU
  aux_minibatch_size: ${run.minibatch_size}
  aux_iterations: 10
  id: ???
  precision: float32
  profile: false
  output_dir: output/${framework.name}/${network.name}/${run.id}/
  minibatch_size: 2
mode:
  optimizer: adam
    loss_balance_scheme: light

```

## iotest

\(
\text{running number} / \text{iteration} = \text{minibatch} / \text{rank}\\


\text{throughput} = \frac{\text{all running number}}{\text{runing time}}\\
\text{throughput} = \frac{\text{running number} / \text{iteration}\times \text{iteration}}{\text{iteration}\times\text{average runing time}}\\
=\frac{ \text{minibatch} / \text{rank}}{\text{average runing time}} \\
=\frac{\text{minbatch}}{\text{rank}\times({\text{reading time + compute time})}} 
\)

