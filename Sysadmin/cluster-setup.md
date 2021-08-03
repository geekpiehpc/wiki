<!-- TITLE: Cluster Setup -->
<!-- SUBTITLE: Cluster setup procedure -->

# Cluster Setup 流程

完整流程 & 踩坑笔录

## 机器信息 & 硬件准备

- 节点：4 节点 （node1~4，node1 为 *主节点*）
- 网络：Ethernet（`192.168.<A>.x`）与 IB（`192.168.<B>.x`）
    - 星状拓扑
    - Setup 时主节点需连接外网
- 硬盘：每个节点一块系统盘，主节点额外挂一块 SSD 作为共享存储
- Clonezilla 镜像 U 盘 * 1（镜像直接解压即可，故下述安装时需要 BIOS 设置为 UEFI 模式）
- Clean Minimal CentOS7 镜像 U 盘 * 1（同上）

## CentOS 操作系统安装

下载 **CentOS-7 Minimal 镜像**，地址：[http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1810.iso](http://isoredirect.centos.org/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1810.iso) 于 U 盘，插于主节点

> 如果主板 BIOS 启动模式不是 UEFI 则勿忘在启动时修改 ;(
> 主节点需要使用外置 Clonezilla 镜像 U 盘，故也把 U 盘启动顺序置前

主节点开机 “install CentOS 7”

> 如果 Install 后触发了 `dracut-init... timeout`，在之后会跳入 `dracut` 命令行，输入 `lsblk` 后找到 U 盘设备，记下 `LABEL=AAA` 的值，而后 `reboot`；然后在选择界面按 `e`，修改第二行中的 `LABEL=BBB` 的第一段 为 `AAA`，然后 `ctrl+x` 即可
> 另一种方法是将LABEL=CentOS\x207\x20x\86_64修改为LABEL=CentOS\x207\x20x\8
> https://blog.csdn.net/qq_36937234/article/details/82996998
> https://access.redhat.com/solutions/2515741

需调整项如下：

- 磁盘分区 `/` + `/boot` 即可，根目录各子目录不分散分区，格为ext4
- 主机名 Hostname 设为 `node1`

待安装完成，以 `root` 用户可正常登陆

**关闭 SELinux**：修改 `/etc/selinux/config`，设置 `SELINUX=disabled`

**关闭 Firewall 防火墙**：

```bash
systemctl stop firewalld.service
systemctl disable firewalld.service
```

> 很多问题都会由上述两个安全服务引起，在超算比赛内网环境下无用，先全关闭

## 以太网连接配置

先配置主节点连接外网，再将各节点内网连接

### Ethernet 外网

连接外网以太网线（记住对应网口 `<INTERFACE>`，e.g. `eno2`）

使用 `ip` 指令检查 DNS 地址等信息，而后在输入 `nmtui` 进入 GUI 网络设置界面，设置外网连接为 DHCP 模式，填入 
DNS 服务器地址，而后使用 `curl` 进行校网登录：

```bash
$ dhclient -v <INTERFACE>
$ curl -X POST --data “userName=<USERNAME>&password=<PASSWD>&hasValidateCode=false&authLan=zh_CN” https://10.15.44.172:8445/PortalServer//Webauth/webAuthAction\!login.action
```

此时可以连接外网，需要记录下本机 ip 地址以便远程连接（校网中途不关机则 DHCP ip 地址应该不会改变）；`curl <URL>` 检查外网连接

### Ethernet 内网

同样使用 `ip` 工具看到网关地址等信息，使用 `nmtui` GUI 工具对内网网口（e.g. `eno1`）进行配置，e.g. 主节点 `192.168.<A>.1`

## 驱动下载 & 安装

IB 驱动 & Nvidia 驱动，安装在默认位置（因为共享盘还未配置），故在拷盘前做好

### IB 驱动和配置

[TODO] [https://blog.csdn.net/oPrinceme/article/details/51001849](https://blog.csdn.net/oPrinceme/article/details/51001849)

### Nvidia 驱动

`yum install kernel-dev epel-release dkms` 来添加 `Redhat` 源 及 其他 Nvidia 驱动依赖

关闭默认 `nouveau` 显卡驱动：

```bash
$ vi /etc/default/grub    # `GRUB_CMDLINE_LINUX` 选项中添加 `nouveau.modeset=0`
$ grub2-mkconfig -o /boot/efi/EFI/centos/grub.cfg
$ reboot
```

> 不要用官网的 rpm 包安装驱动 ;(

重启后，在官网查询所用卡对应的最新驱动版本 `<VER.SUB>`（e.g. V100 目前最新为 `410.79`），获取安装脚本并安装：

```bash
$ wget http://us.download.nvidia.com/XFree86/Linux-x86_64/<VER.SUB>/NVIDIA-Linux-x86_64-<VER.SUB>.run
$ bash NVIDIA-Linux-x86_64-<VER.SUB>.run --kernel-source-path /usr/src/kernels/xxx    # 若报错加此选项安装试试
```

试 `nvidia-smi` 指令看能否获取到显卡信息

## 克隆创建子节点

先安装必要的基本工具以减少重复工作：`yum -y install <TOOL-NAME>`：

- NFS：`nfs-utils rpcbind`
- Lmod：`environment-modules`
- 其他：`gcc gcc-c++ perl wget`（通过 `yum` 预安装 gcc 用于并行库工具等的编译安装）

主节点关机，插入 *Clonezilla* 工具盘，从它启动，将主节点系统盘克隆至子节点系统盘内（勿搞错拷贝 Source & Target 盘方向）：[https://www.tecmint.com/linux-centos-ubuntu-disk-cloning-backup-using-clonezilla](https://www.tecmint.com/linux-centos-ubuntu-disk-cloning-backup-using-clonezilla)

子节点插入系统盘后，分别登陆各子节点，修改主机名和静态 ip 地址（内网网口），以便互联识别身份，注意 **4 节点 ip 和 主机名 互不相同**：

```bash
# e.g. node2 节点
$ hostnamectl set-hostname node2
$ vi /etc/sysconfig/network-scripts/ifcfg-<INTERFACE>   #修改 IPADDR=192.168.<A>.2
```

## 数据盘 NFS 共享（over IB RDMA）

[TODO] [https://community.mellanox.com/s/article/howto-configure-nfs-over-rdma--roce-x](https://community.mellanox.com/s/article/howto-configure-nfs-over-rdma--roce-x)

Maybe useful according to teacher Zhang
```
opensmd
openibd
```

## 数据盘 NFS 共享（over TCP）备选

主节点插上用作共享盘的硬盘，`lsblk` 查看新硬盘已插上及名称，可看到出现 e.g. `sdb1` 盘（根据大小判断那个为共享盘，勿搞错）

> 格式化磁盘流程备忘：
> 
> `$ fdisk /dev/sdb1`
> `$     n                     # 新建分区`
> `$     p 1 [Enter] [Enter]   # 整个盘建立为一个大主分区`

挂载该磁盘并在其中建立欲共享的目录（`/home` 和 `/opt`）:

```bash
$ mount /dev/nvme0n1 /mnt/nfs
$ mkdir /mnt/nfs/home
$ mkdir /mnt/nfs/opt
```

主节点启动 NFS server，编辑共享目录配置 `/etc/exports`，添加条目（*注意不多加空格*）：

```
/mnt/nfs/home 192.168.<A>.0/24(rw,no_root_squash,no_all_squash,sync)
/mnt/nfs/opt  192.168.<A>.0/24(rw,no_root_squash,no_all_squash,sync)
```

参数解释：

- `rw`：可读写
- `no_*_squash`：客户节点以 * 身份使用时不降级为匿名普通用户
- `sync`：各端的写操作同步至磁盘

开启服务并设置开机自启：

```bash
$ exportfs -r
$ service rpcbind start
$ service nfs start
$ chkconfig rpcbind on
$ chkconfig nfs on
```

设置主节点防火墙允许 NFS 访问请求：

```bash
$ firewall-cmd --permanent --add-service=mountd
$ firewall-cmd --permanent --add-service=nfs
$ firewall-cmd --permanent --add-service=rpc-bind
$ firewall-cmd --reload
```

修改 `/etc/fstab`，使主节点将共享目录 *bind mount* （目录树到目录树挂载） 到 `/home` `/opt`，子节点由 NFS 将主节点目录挂载：

```
# On node1，在 /etc/fstab 文件末尾添加
/dev/nvme0n1    /mnt/nfs    ext4    rw,user,exec,suid,dev,auto,async
/mnt/nfs/home   /home       none    rw,user,exec,suid,dev,auto,async,bind
/mnt/nfs/opt    /opt        none    rw,user,exec,suid,dev,auto,async,bind

# On node2~4，在 /etc/fstab 文件末尾添加
node1:/mnt/nfs/home    /home   nfs     rw,user,exec,suid,dev,auto,async
node1:/mnt/nfs/home    /opt    nfs     rw,user,exec,suid,dev,auto,async
```

而后每次开机后，各节点均登入 root 用户，**先在主节点** `mount -a`，**后在各子节点** `mount -a` 即可成功挂载共享目录

> 全手动挂载方式备忘，开机后首先在主节点：
> 
> `$ mount /dev/nvme0n1 /mnt/nfs`
> `$ mount --bind /mnt/nfs/home /home`
> `$ mount --bind /mnt/nfs/opt /opt`
> 
> 而后在各子节点：
> 
> `$ showmount -e node1     # 检测是否有来自主节点的 nfs 共享`
> `$ mount -t nfs node1:/mnt/nfs/home /home`
> `$ mount -t nfs node1:/mnt/nfs/opt  /opt`

> 出现 “Stale file handle” 问题 / “Access denied” 问题，在主节点重启 NFS：`systemctl restart nfs` 后再挂载一遍即可

## SSH 免密码登录配置

首先配置 root 用户相互 *ssh* 免密登陆，**所有节点对之间均需配置**，e.g. 在主节点 `/root` 下：

```bash
$ ssh-keygen            # 位置名称默认
$ ssh-copy-id node1    # 自身节点也需拷贝
$ ...
$ ssh-copy-id node4
```

而后在各自节点 *均* 创建普通用户，注意 **相同名称** & **相同 uid** & **相同 group (gid)** & **相同密码**：

```bash
$ useradd <USERNAME> -m
$ passwd <USERNAME>
$     [Type new PASSWORD] [Type again]    # 设置密码，不要通过 useradd 的 -p 选项，密码不规范时会失败
```

> 密码通过 `passwd` 指令设置，否则密码不规范时 `-p` 选项可能失败且不会给出提示
> 按相同顺序创建即是，可以通过 `cat /etc/passwd` 检查

任意节点进入普通用户，生成并拷贝密钥（注意普通用户 Home 目录共享）：

```bash
$ su testuser
$ cd
$ ssh-keygen
      [Enter] [Enter] [Enter]
$ ssh-copy-id localhost
```

## 编译器、并行库和环境的安装

环境安装目录文件树放置于 `/opt` 下

> 所需环境及安装流程 - 见 “[Environment Installation](Sysadmin/environment-installation.md)”

## 环境 Environment Modules 配置

前面已下载过 Lmod 工具；共享盘中 `mkdir /opt/modulefiles` 作为 modulefile 存储位置，而后在 **每个** 节点固定 modulefile 搜索路径，于 `/etc/environment` 中添加行：

```bash
export MODULEPATH=/opt/modulefiles
```

> 勿忘 `source /etc/environment`

> 曾用 modulefile 文件 - 见 “[Modulefile Records](Sysadmin/environment-modules.md)”
