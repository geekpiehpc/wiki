---
title: Niagara
description: 
published: true
date: 2022-03-26T13:18:25.066Z
tags: 
editor: markdown
dateCreated: 2022-03-26T13:18:22.418Z
---

# Niagara

Used in [ISC21](../../Apps/ISC/ISC-21/README.md)

Login is the same as training nodes, the opened part is only Cascade lake and Ice lake, since the HPC/HPCG/HPCC requires both CPU and GPU, plz affine tasks to those nodes, normally after `gia1000`.

```bash
$ ssh -Y lclmaoroph@niagara.scinet.utoronto.ca
Warning: Permanently added 'niagara.scinet.utoronto.ca' (RSA) to the list of known hosts.
Password: 
===============================================================================
SciNet welcomes you to the NIAGARA supercomputer.

This is a Niagara login node. Use this node to develop and compile code,
to run short tests, and to submit computations to the scheduler.

Remember that /scratch is never backed-up.

Documentation: https://docs.scinet.utoronto.ca/index.php/Niagara_Quickstart
Support:       support@scinet.utoronto.ca or niagara@computecanada.ca
===============================================================================
lclmaoroph@nia-login06:~$ 

```

Filesystem is GPFS, an IBM initiated FS that do not have full POSIX support, with only eventual consistency. But it's real fast for write and can scale up to 50PB.

SCRATCH Area is CVFS, a temporary fast cache for SCRATCH scripts.

The node start up script requires have some problem, no `echo "bla"` in .bashrc but `echo "bla" 1>&2`. PBS requires the bash to hack who you are. Maybe you could hack the quota or raise root permission. We attempted to use [middleware FS](https://github.com/geekpiehpc/plfs-core) and use bashrc to mount on the alocated nodes and eventually make it happen.