 Linux 系统的目录结构遵循（Filesystem Hierarchy Standard，文件系统层次结构标准），所有目录都从根目录开始，形成一个树状结构。这种统一的结构让不同 Linux 发行版（如 Ubuntu、CentOS、Debian 等）的目录布局保持一致，方便用户和程序定位文件。
## 核心目录及其功能
### 1. 系统基础与启动相关
- **`/`（根目录）**  
  整个文件系统的起点，所有目录和文件都挂载在根目录下。

- **`/boot`**  
  存放系统启动必需的文件，包括 Linux 内核（`vmlinuz`）、引导加载程序（如 GRUB 的配置文件 `grub.cfg`）、启动镜像等。系统启动时，BIOS/UEFI 会从这里加载内核。

- **`/sbin`**  
  存放**系统管理命令**，仅 root 用户（超级管理员）常用，用于系统维护，如 `reboot`（重启）、`fdisk`（磁盘分区）、`ifconfig`（网络配置，部分发行版已被 `ip` 替代）等。
### 2. 系统配置与核心文件
- **`/etc`**  
  存放**系统和应用程序的配置文件**，是 Linux 中最常用的目录之一。例如：  
  - 网络配置：`/etc/network/interfaces`（网络接口配置）、`/etc/resolv.conf`（DNS 服务器）；  
  - 用户配置：`/etc/passwd`（用户账户信息）、`/etc/group`（用户组信息）；  
  - 服务配置：`/etc/systemd/system`（systemd 服务配置）、`/etc/nginx`（Nginx 服务器配置）。

- **`/lib` 与 `/lib64`**  
  存放系统运行必需的**共享库文件**（类似 Windows 的 DLL 文件）和内核模块（`.ko`）。  
  - `lib` 对应 32 位系统或 64 位系统的兼容库；  
  - `lib64` 仅在 64 位系统中存在，存放 64 位库文件。
### 3. 设备与虚拟文件系统
- **`/dev`**  
  存放**设备文件**（Linux 中“一切皆文件”，硬件设备通过文件抽象）。例如：  
  - 硬盘：`/dev/sda`（第一块 SATA 硬盘）、`/dev/nvme0n1`（第一块 NVMe 硬盘）；  
  - 终端：`/dev/tty1`（第一个虚拟终端）；  
  - 光驱：`/dev/cdrom`。

- **`/proc`**  
  虚拟文件系统（不占用磁盘空间，数据存于内存），反映**系统实时状态**。例如：  
  - 进程信息：`/proc/1234`（PID 为 1234 的进程详情）；  
  - 系统资源：`/proc/meminfo`（内存使用情况）、`/proc/cpuinfo`（CPU 信息）。

- **`/sys`**  
  虚拟文件系统，主要用于**硬件设备的配置与状态查询**，比 `/proc` 更侧重硬件细节。例如：`/sys/class/net`（网络接口信息）、`/sys/devices`（设备树）。


### 4. 用户与应用相关
- **`/home`**  
  普通用户的**家目录**，每个用户在这里有独立的子目录（如 `/home/zhang`、`/home/lisi`），用于存放用户的个人文件（文档、下载、配置等）。
- **`/root`**  
  超级管理员（root 用户）的家目录，与普通用户的 `/home` 分开，权限更高。
- **`/bin`**  
  存放**基础用户命令**，所有用户均可访问，且系统启动时就需要这些命令（如 `ls` 列表、`cp` 复制、`mkdir` 创建目录等）。
- **`/usr`**  
  存放“用户可共享的程序和数据”，是系统中最大的目录之一，内部结构类似根目录：  
  - `usr/bin`：非启动必需的用户命令（如 `gcc` 编译器、`python` 解释器）；  
  - `usr/sbin`：非启动必需的系统命令（如 `apache2` 服务器启动命令）；  
  - `usr/lib`：应用程序的库文件；  
  - `usr/share`：共享数据（如文档、图标、字体等）；  
  - `usr/local`：用户手动安装的软件（如源码编译的程序，避免与系统自带软件冲突）。
- **`/opt`**  
  存放**第三方可选应用**（通常是大型软件），如 Oracle 数据库、Matlab 等，方便集中管理和卸载。

### 5. 临时与可变数据
- **`/tmp`**  
  临时文件目录，所有用户可读写，系统会定期自动清理（或重启后清空），适合存放临时缓存、日志等。

- **`/var`**  
  存放**“可变数据”**（内容随系统运行动态变化）：  
  - `var/log`：系统和应用程序的日志文件（如 `syslog` 系统日志、`auth.log` 登录日志）；  
  - `var/cache`：应用程序的缓存（如浏览器缓存、软件包缓存）；  
  - `var/mail`：用户邮件；  
  - `var/spool`：队列数据（如打印任务、定时任务）。


### 6. 外部存储挂载
- **`/mnt`**  
  手动挂载外部存储设备（如 U 盘、移动硬盘、网络共享目录）的临时挂载点，例如 `mount /dev/sdb1 /mnt/usb` 可将 U 盘挂载到 `/mnt/usb`。

 - **`/media`**  
  自动挂载外部设备的目录（现代 Linux 发行版常用），插入 U 盘或光盘时，系统会自动在 `/media/用户名/设备名` 下创建挂载点。


## 总结
Linux 目录结构的设计遵循“功能分离”原则：系统文件与用户文件分离、静态文件（如程序）与动态文件（如日志）分离、必需文件与可选文件分离。理解这些目录的作用，能帮助你快速定位文件、排查问题（如日志在 `/var/log`，配置在 `/etc`），是使用 Linux 的基础。
## linux中ubuntu安装软件操作
### 通过APT安装包安装
- sudo apt update
- sudo apt install firefox
- sudo apt remove firefox
- sudo apt upgrade  # 升级所有可更新的软件
- sudo apt upgrade firefox  # 仅升级指定软件
- apt search [关键词]  # 例如查找 "编辑器" 相关软件：apt search editor
### 源码编译安装
安装编译工具（首次编译需执行）：
- bash
- sudo apt install build-essential  # 包含 gcc、make 等基础编译工具
下载并解压源码包：
例如下载 example-1.0.tar.gz 后，在终端解压：
- bash
- tar -zxvf example-1.0.tar.gz  # 解压 gz 格式
- cd example-1.0  # 进入解压后的目录
配置编译参数：
大多数源码包会提供 configure 脚本，用于检查依赖并生成 Makefile：
- bash
./configure  # 可添加参数，如指定安装路径：./configure --prefix=/usr/local
编译源码：
- bash
- make  # 编译过程可能耗时，取决于软件复杂度
安装编译好的软件：
- bash
- sudo make install
卸载源码安装的软件（需在源码目录执行）：
- bash
- sudo make uninstall

