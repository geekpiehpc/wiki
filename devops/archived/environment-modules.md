---
title: Environment Modules
published: true
date: 2022-03-26T13:18:33.561Z
tags: null
editor: markdown
dateCreated: 2022-03-21T02:21:34.280Z
description: Environment Module Management & Backups
---

# Environment Modules

> **Warning** This is an outdated guide.

Environment Module: Modulefiles 目录树结构 + 备份

## Modulefiles 目录树结构 （Deprecated）

```bash
|- /opt/modulefiles    # 路径并非完全按照冲突关系组织，modulefile 中冲突关系要注意
    |- mpi/
        |- openmpi/
            |- 4.0
            |- 3.1
            |- ...
        |- mpich/
        |- intelmpi/
    |- math/
        |- mkl/
        |- blas/
    |- compilers/
        |- gcc/
        |- icc/
        |- ifort/
    |- cuda/
        |- nvidia/
        |- pgi/
    |- netcdf/
        |- pnetcdf/
        |- netcdf-c/
        |- netcdf-fort/
```
