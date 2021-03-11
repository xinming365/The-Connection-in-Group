
 # DIY 深度学习服务器

由于组内国家重点研发计划项目中，明确写了需要使用部分经费搭建一台计算用服务器，所以只能按照计划，使用经费自己搭建服务器。近期我们组研究方向朝着机器学习方向发展，
因此，我们决定准备购置GPU搭建深度学习用的服务器。

## 硬件设施配置
- 显卡 GeForce RTX 2080  7块。显存8G 
- 主板  2×PCIe，至少8通道，（考虑16×）用于GPU显卡 1×PCIe用于Infiniband卡，2个千兆网络接口，HDD/SSD存储
- CPU Xeon处理器.供参考的 X399 或者7900X主板
- 内存  4×16G 四通道内存/双通道内存  2×32G。 频率300MHz，考虑主板支持频率
- 硬盘 SSD NVME M.2 + 普通硬盘 1T
- 电源（PSU） 650w以上
- 机箱 尺寸2U以内，方便放在物理系的机房机架上。
- 风扇
- 网卡 Mellanox NIC(Network Interface Controller)  PCIe接口 （ Mellanox connectX-3 10Gbps NIC）用于内部机器数据交换。 Intel 2*1Gbps AM350 网卡。
：GigaBits or GigaBytes per second （b for bits and B for Bytes)

## 硬件安装
- 电源连接线。连接主板（motherboard）， 风扇， CPU（2*8引脚），2*SATA（HDD，SSD）。
- cpu冷却器（散热）。cpu。内存条安装。512GB的NVMe 960 pro。
- 管理一下线缆的放置。
- 硬盘器托架（drive bays），5*sata线。4*12TB的 seagate机械硬盘。三星860evo固态硬盘。螺丝安装不上可以使用双面胶带。
- x370-pro 主板，8个SATA接口

- 硬盘器托架（drive bays），5*sata线。4*12TB的 seagate机械硬盘。三星860evo固态硬盘。螺丝安装不上可以使用双面胶带。
- 检查所有的硬件是否被检测到，启动电脑之前更新bios
## 物理部署

## 软件环境
集群的每个节点（主节点，登陆节点，计算节点）都需要有操作系统。操作系统可以安装在节点的硬盘驱动器上。
计算节点，存储节点，管理节点，集群辅件。
### 操作系统
- Ubuntu系统 :去Ubuntu官网下载server版本的系统：https://ubuntu.com/download/alternative-downloads 然后制作Ubuntu server的U盘启动盘。可以通过Rufus工具创建USB启动盘，网站在https://rufus.ie/ 插入镜像文件。
- 或者CentOS系统：https://www2.physik.uni-bielefeld.de/gpu-cluster.html
### 驱动和开发工具

基于TCP或者其它协议的，通常是InfiniBand或者其余的高速网络
#### Infiniband驱动
- 去官网上找到适合自己硬件的驱动：https://www.mellanox.com/products/InfiniBand-VPI-Software 安装教程如下：https://blog.csdn.net/oPrinceme/article/details/51001849
#### 并行计算开发环境驱动
- Intel编译器。需要下载并安装intel parallel studio xe 2015，包含性能分析工具，编译器，高性能库，并行工具等等。下载链接：https://software.intel.com/content/www/us/en/develop/tools/parallel-studio-xe/choose-download.html
- Intel 多核堆栈平台（MPSS：Manycore Platform Software Stack)用于intel Xeon Phi 协处理器 https://software.intel.com/content/www/us/en/develop/articles/intel-manycore-platform-software-stack-mpss.html


- CUDA环境

### 节点信息存储系统
安装NFS（Network File System）网络文件系统，是一种分布式的文佳你系统。

### 网络信息服务
安装NIS （Network Information System），集中控制几个系统管理数据库的网络用品。需要在所有机器上安装。

## HPC系统网络
### 外部网络
集群不同主机网线接入交换机之后，首先需要对网络进行设置。编辑/etc/hosts设置ip和域名的映射关系。（dns缓存>hosts>dns服务）。hosts文件的格式为:ip  主机名/域名。在该文件中添加下面的内容。
对不同节点设置私有ip地址：
|  主机名   | IP地址  |
|  ----  | ----  |
| 计算节点node01  | 192.168.1.1 |
| 计算节点node02  | 192.168.1.2 |
| 计算节点node03  | 192.168.1.3 |
| 计算节点node04  | 192.168.1.4 |

设置计算节点的私有ip地址。打开文件:

对物理系的网络环境，需要设置控制节点的子网掩码，默认网关，dns服务器，静态ip，子网掩码，才能与外界通信。

删除计算节点的普通用户。userdel –r <your username>。下面设置通过控制节点无密码登陆各个计算节点。
 
### NFS设置
需要设置不同机器的文件共享。打开/etc/exportfs文件，添加：
/home  192.168.1.0/255.255.255.0(rw,sync,no_root_squash,subtree_check) 
/opt   192.168.1.0/255.255.255.0(rw,sync,no_root_squash,subtree_check) 
/usr/local  192.168.1.0/255.255.255.0(rw,sync,no_root_squash,subtree_check)


共享/home目录，用户存储，这样文件才能够被计算节点获取。/opt为了共享intel编译器，/usr/local为了共享各种安装的其他软件。
192.168.1.0/255.255.255.0表示可以挂载的ip地址范围。表示共享到所有192.168.1.X的IP。(rw,sync,no_root_squash,subtree_check)表示挂载的配置，这里表示可读可写，同步执行，若 NFS 主机使用分享目录的使用者，如果是 root 的话，那么对 于这个分享的目录来说，他就具有 root 的权限，强制 NFS 检查父目录的权限。

### NIS设置
配置用户信息。这样客户端可以利用NIS服务管理的文件信息，而不是用自身的管理文件（/etc/passwd）。这样只需要维护在NIS服务器的文件。在计算节点安装时，需要确认域名为控制节点名称。在/etc/defaultdomain（包含 NIS 域名的文件）文件中查看。

### 计算网络
## 主节点

## 计算机节点

## 管理和监视

学习参考链接
https://medium.com/the-mission/how-to-build-the-perfect-deep-learning-computer-and-save-thousands-of-dollars-9ec3b2eb4ce2
https://l7.curtisnorthcutt.com/build-pro-deep-learning-workstation
http://mli.github.io/gpu/2016/01/17/build-gpu-clusters/
搭建集群CSDN：https://blog.csdn.net/gugugujiawei/article/details/44592049
## 国内的超算中心
上海交通大学Pi：https://hpc.sjtu.edu.cn/Item/Software.htm



