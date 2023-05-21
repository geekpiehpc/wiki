---
title: NVIDIA Repositories
description: 
published: true
date: 2022-12-16T08:59:29.564Z
tags: cuda
editor: markdown
dateCreated: 2022-12-16T08:17:35.672Z
---

# NVIDIA Repositories

You can always replace developer.download.nvidia.com with developer.download.nvidia.cn for faster speed.

## MLNX_OFED

See https://network.nvidia.com/support/mlnx-ofed-public-repository/

- You can find repo file at https://linux.mellanox.com/public/repo/mlnx_ofed/latest/ubuntu22.10/mellanox_mlnx_ofed.list
- Instructions to install packages: https://network.nvidia.com/pdf/prod_software/Using_MLNX_OFED_Repository.pdf.

## CUDA

See https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#package-manager-installation


1. Install the new cuda-keyring package

    ```bash
    wget https://developer.download.nvidia.com/compute/cuda/repos/$distro/$arch/cuda-keyring_1.0-1_all.deb
    sudo dpkg -i cuda-keyring_1.0-1_all.deb
    ```

2. Update the Apt repository cache

    ```bash
    sudo apt-get update
    ```

3. Install CUDA SDK

    > These two commands must be executed separately.

    ```bash
    sudo apt-get install cuda
    ```

    ```bash
    sudo apt-get install nvidia-gds
    ```

4. Reboot the system

    ```bash
    sudo reboot
    ```

## Container runtime

See https://nvidia.github.io/nvidia-container-runtime/

```bash
curl -s -L https://nvidia.github.io/nvidia-container-runtime/gpgkey | sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-container-runtime/$distribution/nvidia-container-runtime.list | sudo tee /etc/apt/sources.list.d/nvidia-container-runtime.list
sudo apt-get update
```

For pre-releases, you need to enable the experimental repos of all dependencies:

```bash
sudo sed -i -e '/experimental/ s/^#//g' /etc/apt/sources.list.d/nvidia-container-runtime.list
sudo apt-get update
```

## HPK SDK (NVHPC)

See https://developer.nvidia.com/nvidia-hpc-sdk-downloads

### Bundled with the newest CUDA version

```bash
curl https://developer.download.nvidia.com/hpc-sdk/ubuntu/DEB-GPG-KEY-NVIDIA-HPC-SDK | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-hpcsdk-archive-keyring.gpg
echo 'deb [signed-by=/usr/share/keyrings/nvidia-hpcsdk-archive-keyring.gpg] https://developer.download.nvidia.com/hpc-sdk/ubuntu/amd64 /' | sudo tee /etc/apt/sources.list.d/nvhpc.list
sudo apt update
sudo apt install nvhpc-xx-xx
```

### Bundled with the newest plus two previous CUDA versions

```bash
curl https://developer.download.nvidia.com/hpc-sdk/ubuntu/DEB-GPG-KEY-NVIDIA-HPC-SDK | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-hpcsdk-archive-keyring.gpg
echo 'deb [signed-by=/usr/share/keyrings/nvidia-hpcsdk-archive-keyring.gpg] https://developer.download.nvidia.com/hpc-sdk/ubuntu/amd64 /' | sudo tee /etc/apt/sources.list.d/nvhpc.list
sudo apt-get update
sudo apt-get install nvhpc-xx-xx-cuda-multi
```
