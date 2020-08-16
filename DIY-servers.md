
 # DIY 深度学习服务器

由于组内国家重点研发计划项目中，明确写了需要使用部分经费搭建一台计算用服务器，所以只能按照计划，使用经费自己搭建服务器。近期我们组研究方向朝着机器学习方向发展，
因此，我们决定准备购置GPU搭建深度学习用的服务器。

## 硬件设施配置
- 显卡 GeForce RTX 2080  7块。显存8G
- 主板  2×PCIe，至少8通道，（考虑16×）用于GPU显卡 1×PCIe用于Infiniband卡，2个千兆网络接口，HDD/SSD存储
- CPU Xeon处理器.供参考的 X399 或者7900X主板
- 内存  4×16G 四通道内存/双通道内存  2×32G。 频率300MHz，考虑主板支持频率
- 硬盘 SSD NVME M.2 + 普通硬盘 1T
- 电源 650w以上
- 机箱 尺寸2U以内，方便放在物理系的机房机架上。
- 风扇

## 软件环境
集群的每个节点（主节点，登陆节点，计算节点）都需要有操作系统。操作系统可以安装在节点的硬盘驱动器上。
### 操作系统
- Ubuntu系统 :去Ubuntu官网下载server版本的系统：https://ubuntu.com/download/alternative-downloads 然后制作Ubuntu server的U盘启动盘。可以通过Rufus工具创建USB启动盘，网站在https://rufus.ie/ 插入镜像文件。
- 或者CentOS系统：https://www2.physik.uni-bielefeld.de/gpu-cluster.html
### 驱动和开发工具

- CUDA环境

计算节点，存储节点，管理节点，集群辅件。
## 物理部署

## HPC系统网络
### 外部网络
基于TCP的管理网络
### 计算网络
基于TCP或者其它协议的，通常是InfiniBand或者其余的高速网络
#### Infiniband驱动
- 去官网上找到适合自己硬件的驱动：https://www.mellanox.com/products/InfiniBand-VPI-Software 安装教程如下：https://blog.csdn.net/oPrinceme/article/details/51001849

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



