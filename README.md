# The-Connection-in-Group

我们组目前计算的服务器目前有下面几个地方：

- 自己搭建的工作站
- 学校的集群
- 并行科技（校外服务商）的服务器
- 天津超算

The essential information of the connections to the computing resources
## 工作站的连接
Ip地址为：166.111.26.46
注意必须使用校内网连接。（DIVI，DIVI2不能使用）

## 学校集群的连接
### Windows
Use the IE explorer and open the website https://166.111.143.19:10000/svpn/vpnuser/home.cgi, which is the the SSL VPN for the login.
To use it, you need to install the ActiveX . 

如果安装后，不能够正确加载该控件。可以通过下面的步骤启用

浏览器右上角设置-internet选项-安全-取消勾选 启用保护模式 ，然后再在这个页面依次选择：自定义级别-启用ActiveX控件和插件

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

## 并行科技的连接
下载他们提供的并行超算云服务客户端，自己创建账号和密码，然后跟我说或者在群里向工程师要求，把nijun的服务器资源链接到自己的账号下。

