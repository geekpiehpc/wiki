# CycleCloud

Azure CycleCloud is a tool for deploying HPC clusters in Azure and managing their workloads. 

For a detailed introduction to CycleCloud, see [CycleCloud Introduction](https://docs.microsoft.com/en-us/azure/cyclecloud).
For a step-by-step guide to using CycleCloud, see [Create, customize and manage an HPC cluster in Azure with Azure CycleCloud](https://docs.microsoft.com/en-us/learn/modules/azure-cyclecloud-high-performance-computing/).

## CycleCloud CLI

Install cli from About page of CycleCloud dashboard.
<img width="419" alt="image" src="https://user-images.githubusercontent.com/65301509/140474967-83bde5b7-9df4-4cc1-9b46-37b04adce16a.png">

### Note: Install on Windows

CycleCloud on Windows depends on cryptography package.
So for install script to finish, we need to do this before running script

1. Install OpenSSL
    choco install openssl -pre -y
2. Append `C:\Program Files\OpenSSL-Win64\lib;` to environment variable `LIB`
3. Append `C:\Program Files\OpenSSL-Win64\include;` to environment variable `INCLUDE`
