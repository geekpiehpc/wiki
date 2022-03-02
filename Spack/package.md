# Package

原始环境中常常有各种环境变量，这些环境变量在给 Spack 打包的时候可能会有影响，因此本 SOP 提出了一种用 docker 做环境隔离的方式，以便在不影响原始环境的情况下，可以在不同的环境中运行 Spack 的打包。

## How to run

```bash
$ cd <Dockerfile-Folder>
$ docker build -t <image-name> .
$ docker run -it -d -v <spack-folder-your-machine>:<spack-folder-your-machine> --name <docker-name> <image-name>
$ docker exec -it <docker-name> bash
```

## Dockerfile ( Long-Term Support )

```Dockerfile
FROM ubuntu:20.04
RUN apt-get update
RUN apt-get install -y ca-certificates
RUN sed -i "s/archive.ubuntu.com/mirrors.shanghaitech.edu.cn/g" /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y python python3 gcc build-essential wget nano vim gfortran curl
```
