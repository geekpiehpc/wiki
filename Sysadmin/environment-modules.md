<!-- TITLE: Environment Modules -->
<!-- SUBTITLE: Environment Module Management & Backups -->

# Modulefile 记录

Environment Module: Modulefiles 目录树结构 + 备份

## Modulefiles 目录树结构

```
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

## 备份记录