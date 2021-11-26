# CycleCloud

Azure CycleCloud is a tool for deploying HPC clusters in Azure and managing their workloads.

For a detailed introduction to CycleCloud, see [CycleCloud Introduction](https://docs.microsoft.com/en-us/azure/cyclecloud).
For a step-by-step guide to using CycleCloud, see [Create, customize and manage an HPC cluster in Azure with Azure CycleCloud](https://docs.microsoft.com/en-us/learn/modules/azure-cyclecloud-high-performance-computing/).

虽然我不想说……但最好的方法还是先去看看官方文档，了解一下它的功能，还有他的模板怎么写……新概念可能比较多，可以一边操作一边学习。
但请注意他的文档不全，或者有些内容过时了。如果你想确认最新的 CycleCloud 行为，用它开一台机器，把上面 `/opt/cycle` 的东西下下来，看里面的代码，是最直接的方法了。

## Introduction: ...So what is CycleCloud?

……想说请这个先讲点历史。

CycleCloud 本来属于 [Cycle Computing](https://en.wikipedia.org/wiki/Cycle_Computing)，后来被微软收购了。在被微软收购以前，CycleCloud 是可以在很多平台上使用的，包括 Amazon Web Services, Google Compute Engine 甚至是公司内部集群。

它的作用就是帮你方便的管理一堆 HPC 资源，举个例子我现在想在 AWS 上开 15 台机器作为我的 HPC Cluster，正常情况下我可能是一台一台开，聪明点的人知道写个脚本，**一次申请所有资源**。

不过就算这样你可能还要对每台机器做一些**初始化**，比如配置网卡、软件、用户等等。当然现在有一些高级工具比如 [Cloud-Init](https://cloudinit.readthedocs.io/en/latest/) 可以让你更方便的完成这些初始化工作，不过用它配置一堆软件还是显得有些吃力。
比较现代的解决方法是是用 [Ansible](https://www.ansible.com/)，通过它和一个包含“节点内所有机器”的文件，它可以自动帮你去做各种奇奇怪怪的初始化工作（实际写起来有点像 GitHub Actions）。

还有一件事情就是你需要**监控**这些机器是否正常，如果不正常可能要把一些机器移除。此外可能也要根据工作负载**动态调节**(autoscaling)你的机器数量。这些云服务不一定会帮你做，虽然说动态调节会让人想到 k8s，但用 k8s 跑 HPC 可能现在还是有点要命吧？

说了这么多，**CycleCloud 就是这么个工具，它帮你自动化的控制云上的 HPC 资源，让你只要点几下就可以建立一个*好用、稳定*的 HPC 集群。**
> ……😢 实际上可能没有那么好用了，不过至少这应该是他们的愿景吧……。

## Prerequisites: What do I need to know to learn it?

### Template

CycleCloud 最重要的概念是模板，它是一个包含了一个**集群**(Cluster)所有软硬件需求的文件。CycleCloud 将根据这个文件创建集群，而你点加号看到的那些 scheduler, filesystem, etc. 都是模板，你甚至可以在 Azure GitHub 里找到这些模板。

模板使用的格式类似 `ini` 但又比 `ini` 高级。如果感觉难以下手的话，先去看看 [toml](https://github.com/toml-lang/toml) 这个东西。

### Cluster-init? Project? 

你会留意到它文档里会提到 `cluster-init` 或者 `project` 这种东西。这两个是同义词。**注意不要和 `cloud-init` 搞混了**，这个与我们现在说的的无关。为了避免混淆，我们之后都用 `project` 这个词。

> `cloud-init` 和 `project` 都是初始化用的，但 `cloud-init` 更底层，由 Azure 负责。`project` 则由 CycleCloud 负责。
> 换句话说，`cloud-init` 跑完以后，你的机器其实已经创建好了，这个时候在被 CycleCloud 做二次初始化，而二次初始化具体来说就是执行一系列的脚本和 Chef Cookbook。

在学习 CycleCloud 的同时你最好去了解一下 [Chef Infra](https://downloads.chef.io/tools/infra-server)，所有的 Project 你剥开以后就是 Chef Cookbooks，而 Chef Cookbooks 和 Ansible Playbooks 差不多，都是用来初始化机器的。
学习 Chef 的时候你会发现你其实在学 Ruby……这也没有办法嘛。Ruby 的语法需要适应，尤其是之前没有接触过的话。另外尤其注意 CycleCloud 所使用的 Chef 版本（你可以在 `/opt/cycle/` 里找到它的二进制文件），避免用了旧版没有的东西。

> **小心**
> CycleCloud Chef 所用的 Ruby 版本对一些 SSL 网站的支援存在问题，见 <https://bugs.ruby-lang.org/issues/15594>。如果要下载东西，可能要用 http 或者自己写个 Chef Resource，在其中调用 Ruby 的 `::URI.open`，并且设置 `ssl_verify_mode` 为 `OpenSSL::SSL::VERIFY_NONE`。

## CycleCloud CLI

Install cli from About page of CycleCloud dashboard.
<img width="419" alt="image" src="https://user-images.githubusercontent.com/65301509/140474967-83bde5b7-9df4-4cc1-9b46-37b04adce16a.png">

## Custom image

There are more images available than CycleCloud built-in images.
And to make GPU work, we must use image with Generation 2 support.

[Azure HPC VM Images](https://techcommunity.microsoft.com/t5/azure-compute-blog/azure-hpc-vm-images/ba-p/977094) contains all images currently available.
Choose one you need, and use it's urn to specify it.

> **Note**
> Some images are incompatible with CycleCloud's built-in templates. For example, you can't use `microsoft-dsvm:ubuntu-hpc:2004:` with [Slurm Template](https://github.com/Azure/cyclecloud-slurm).
> We have a custom template to solve it.

### Note: Install on Windows

CycleCloud on Windows depends on cryptography package.
So for install script to finish, we need to do this before running script

1. Install OpenSSL
    choco install openssl -pre -y
2. Append `C:\Program Files\OpenSSL-Win64\lib;` to environment variable `LIB`
3. Append `C:\Program Files\OpenSSL-Win64\include;` to environment variable `INCLUDE`

## Useful links

- [CycleCloud debian repository](https://packages.microsoft.com/repos/cyclecloud/)
    - [Insiders version](https://packages.microsoft.com/repos/cyclecloud-insiders/)) 
