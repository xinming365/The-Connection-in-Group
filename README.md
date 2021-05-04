# The-Connection-in-Group

我们组目前计算的服务器目前有下面几个地方：

- 自己搭建的工作站
- 学校的集群
- 并行科技（校外服务商）的服务器
- 天津超算

The essential information of the connections to the computing resources
## 工作站的连接
工作站介绍：cpu类型:Intel(R) Xeon(R)  E5-2680 v4; 共28核心，主频2.4GHz；
Ip地址为：166.111.26.46
注意必须使用校内网连接。（DIVI，DIVI2不能使用）
### windows
推荐使用xmanager客户端，设置好账户名称和密码，就能直接使用

### Linux或者Mac
通过加密网络传输工具ssh直接进行远程登陆。使用方式如下：
> ssh user@host
例如
> ssh xinming@166.111.26.46



## 学校集群的连接
学校集群介绍：cpu类型：Intel(R) Xeon(R)  CPU E5620；4核心；主频2.4Ghz
目前每年采用共享模式包年120核心。

登陆方式介绍如下：
### Windows
Use the IE explorer and open the website https://166.111.143.19:10000/svpn/vpnuser/home.cgi, which is the the SSL VPN for the login.
To use it, you need to install the ActiveX . 

如果安装后，不能够正确加载该控件。可以通过下面的步骤启用

浏览器右上角设置-internet选项-安全-取消勾选 启用保护模式 ，然后再在这个页面依次选择：自定义级别-启用ActiveX控件和插件

此外，右上角设置-管理加载项，查看ActiveX控件是否正常加载。

The user account is nijun. The password is 3309_junni

In the Windows platform, you need a software to build connections between your own computer and the remote Linux machines. The Xmanager software is recommended
for you. Xmanager下载链接：https://pan.baidu.com/s/14m1dOhkQu54UByi5rt8CPg 提取码：t5or 。


创建新的一个链接：名称:自己定义，协议（protocol）:ssh，主机（host）：192.168.1.2，端口号（Port number）：22.

登录用户名：nijun，公钥（Public key）-用户密钥，选择相应文件添加进去，文件链接：https://pan.baidu.com/s/1aenni0NoXRQILG5rZIg-0A 提取码：r88s。之后输入登录密码：junni_3309。
创建好之后就能够正常连接超算集群。


### Mac
打开设置-通用-网络-VPN


然后点击左下角加号（+），新建一个网络端口。接口类型VPN，VPN类型：Cisco IPSec，服务器名称：VPN（Cisco IPSec）。确认创建


服务器地址：166.111.143.27
账户名称：nijun
密码：m5Bj01e


点击鉴定设置-输入共享密钥hpccenter,点击连接。
前端机登陆节点地址：192.169.1.2

### 使用介绍
连接上之后，注意所有的计算文件要**放在WORK1目录下**，在WORK1下面呢创建一个自己的工作目录。学校的集群计算平台使用了pbs作业调度系统。pbs的提交脚本示例如下：
```
#!/bin/sh
#BSUB -q hpc_linux
#BSUB -n 12
#BSUB -o task.o%J
#BSUB -e task.e%J
#BSUB -a intelmpi
##BSUB -u xinming_365@163.com
##BSUB -sp 100
NPROCS=12
VASP="/home/nijun/apps/vasp/vasp.5.4.1/bin/vasp_std"
vasppot="/home/nijun/apps/vasp/vasppot/vasppot_paw_pbe.54.sh"
#vasppot="/home/nijun/apps/vasp/vasppot/vasppot_paw_pbe.52.sh"
MPIVASP="mpirun.lsf -np ${NPROCS} ${VASP}"
${vasppot}  Cs_sv  Pb_d Br # 将产生potcar的命令写成了脚本，顺序连接Cs Pb Br的赝势形成POTCAR文件。
# 假设INCAR，KPOINTS，POSCAR已经写好，此处略去。
${MPIVASP} >relax.out
```
假设该脚本保存为run_job.sh文件，通过bsub < run_job.sh命令，就可以将该计算提交上去。

之后可以通过bjobs命令查看作业状态
```
JOBID      USER    STAT  QUEUE      FROM_HOST   EXEC_HOST   JOB_NAME   SUBMIT_TIME
17128621   nijun   RUN   hpc_linux  ln1         12*c28b02   *????????? Oct 15 10:29
```
状态（STAT）为正在运行（RUN），此外还有（PEND正在排队等待，PSUSP任务在队列中排队等待时被用挂起，SSUSP任务被系统挂起，DONE作业正常结束，exit代码为0，EXIT作业退出）

## 并行科技的连接
下载他们提供的并行超算云服务客户端，自己创建账号和密码，然后跟我说或者在群里向工程师要求，把nijun的服务器资源链接到自己的账号下。
### 使用介绍
并行科技的计算资源比较丰富，空闲机器较多。该集群使用了slurm作业调度系统，Slurm提交脚本示例如下
```
#!/bin/bash
#SBATCH -p v3_64
#SBATCH -N 1
#SBATCH -n 24
source /public1/soft/other/vasp/cn-module-vasp.5.4.4.sh
module  unload intel/17.0.7
module load  mpi/intel/5.0.3.049
module load vasp/intel-17/vasp544
srun vasp_std > scf.out
```

该脚本为vasp软件计算提交脚本，假设脚本所在目录已经存在vasp计算所需要的四个文件，则通过使用
> sbatch sub_job.sh

就可以将该任务提交上去。
之后通过squeue命令可以查看目前正在运行的任务，例如squeue后显示：
```
(uspex) [sc30830@ln6%cstc9 ~]$ squeue
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON) 
           1785502     v3_64 run_1_re  sc30830  R 2-02:03:08      1 c3703 
```
这些信息中，显示该计算任务放在v3_64分区上运行，占用一个节点（Nodes 1） 目前状态（ST）为运行状态（R），并且运行时间为：2-02:03:08。
程序完成后，会产生一个slurm-\<jobid\>.out的文件，如果报错可以查看该文件信息。


## 天津超算的连接
略
#### 注意：校内的集群和组内的工作站使用都需要使用校园网，如果在校外的话，可以通过SSL VPN服务线连接上校园网，可通过web方式或者客户端方式（推荐）。安装使用介绍：[https://its.tsinghua.edu.cn/helpsystem/sslvpnhelp/khdfw/khd.htm](https://its.tsinghua.edu.cn/helpsystem/sslvpnhelp/khdfw/khd.htm)
