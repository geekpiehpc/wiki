---
title: it-support-machine
published: true
date: 2022-03-21T03:20:05.883Z
tags: null
editor: markdown
dateCreated: 2022-03-21T02:21:38.829Z
---

# It-Support Machine

## 机器简介

我们一共拥有四个图信账号，分别是：`GeekPieHPC{1, 2, 3, 4}`。 四个账号均位于同一 IP 地址下，[**请到 Slack 查看机器的 IP 地址**](https://geekpiehpc.slack.com/archives/C0210BA22QH/p1631708325019000)。

## 连接方式

这些账号均可使用 `ssh` 命令连接，或使用 `scp` 命令进行文件传输，**端口号为 `22112`**。

四个账号均位于同一 IP 地址下，见 [https://geekpiehpc.slack.com/archives/C0210BA22QH/p1631708325019000](https://geekpiehpc.slack.com/archives/C0210BA22QH/p1631708325019000)。

*   **密钥**: 您可以使用连接到 Epyc 机器的密钥进行登录，示例配置如下

    ```sshconfig
    Host geekpie<数字>
    HostName <在这里填上目标机器的 IP 地址>
    User geekpie<数字>
    Port 22112
    IdentityFile ~/.ssh/id_rsa_epyc
    ```
* **密码**: 您可在 Slack 中找到密码。

## 调度器

图信机器使用 PBS ([Portable Batch System](https://en.wikipedia.org/wiki/Portable\_Batch\_System)) 进行调度，其多数命令以 `q` 开头，如可以使用 `qstat` 查看调度器的状态。

CPU 队列为 `GeekPie_HPC`，GPU 队列为 `GeekPie_GPU`。

PBS 的具体使用方式请看 [DevOps/Scheduler](../../DevOps/Scheduler.md)。

## 环境管理

在图信机器上，首推使用 `module` 进行环境变量的管理，不过和编译器打交道时，还是使用 spack 安装编译器。

## 支持与帮助

可以使用以下方式联系图信

* **微信**: 群中的 Saber 为图信联络人。
* **办公室**: 图信办公室位置是 `H1 304`，H1 楼是医务室那栋楼。
