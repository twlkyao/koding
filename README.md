koding
======

A project to simulate the koding terminal.


#网页版Terminal（WebShell）详解
-------------------------------------------
##技术背景
类似于xshell，secureRT，putty，在运维管理中有很多麻烦，不同的用户，需要不同的登录名，授权等等管理，并且当个人电脑被攻击后存在安全问题。

现有方案：堡垒机、跳板机等一系列安全措施来防止系统非法访问，或者登录系统时需要手机验证码来充分验证用户身份。

跳板机的弱势：

1. 唯有控制才能真正解决问题
2. 系统账号无法确认用户身份
3. 人为操作难免会出问题

由于跳板机的弱势出现了运维堡垒机，运维堡垒机具有的优点：

1. 对运维人员的身份认证
2. 对运维操作的访问控制和审计等


##实现原理

###前端
提示文本渲染到面板中，然后当用户输入命令时，发送ajax请求到服务端，服务端解析命令文本，并且用相应的工具类来与Linux服务器打交道，把处理结果封装在一个json对象返回到客户端，客户端将结果渲染到面板中。  

###后台

[Koding](http://www.koding.com/ "Koding")使用了Linux Container(容器)[LXC](https://github.com/lxc/lxc "LXC")来进行虚拟化（著名的DotCloud的Docker也是构建在LXC上的）。LXC是在Linux的系统层实现的虚拟化，LXC包含了CGroups和命名空间的支持来为应用提供独立的运行环境。  

LXC依靠Linux内核层上的CGroups和其他的命名空间个例的功能，从而可以构建具有独立进程和网络空间的虚拟运行环境。

从LXC1.0版本开始，LXC容器在宿主机上以普通用户的身份运行，从而不能直接访问宿主机的硬件，即使特权容器也提供了恰当的隔离机制，从而确保了容器内的系统不会对宿主机造成安全影响。


##开源软件
1. [GateOne](https://github.com/liftoff/GateOne/) 使用10443端口，使用Python，JavaScript，CSS等开发。
2. [ShellInABox](https://code.google.com/p/shellinabox/) 使用4200端口。
3. [WSSH](https://github.com/aluzzardi/wssh/) 使用5000端口，使用Python，JavaScript，CSS开发。
4. [Web-Shell](http://code.google.com/p/web-shell/) 针对iPhone的WebShell。
5. [LXC](https://github.com/lxc/lxc "LXC") Linux系统层上的容器。

##安全性分析
1. 通信数据难于过滤。
2. 可以通过上传文件，包括“图片”（仅仅是后缀名为图片类型），压缩文件，数据库文件（可以通过select语句生成WebShell，从而提权）。
3. LXC可以在操作系统层次上为进程提供虚拟的执行环境，一个虚拟的执行环境就是一个容器。可以为容器绑定特定的cpu和memory节点，分配特定比例的cpu时间、IO时间，限制可以使用的内存大小（包括内存和是swap空间），提供device访问控制，提供独立的namespace（网络、pid、ipc、mnt、uts），使用比较灵活。

##优点
###前端


###后台
LXC是操作系统层次的虚拟化技术，与传统的HAL（硬件抽象层）层次的虚拟化技术相比有以下优势：

1. 更小的虚拟化开销（LXC的诸多特性基本由内核特供，而内核实现这些特性只有极少的花费）。
2. **快速部署**。利用LXC来隔离特定应用，只需要安装LXC，即可使用LXC相关命令来创建并启动容器来为应用提供虚拟执行环境。传统的虚拟化技术则需要先创建虚拟机，然后安装系统，再部署应用。
3. LXC跟其他操作系统层次的虚拟化技术相比，最大的优势在于LXC被**整合进内核**，不用单独为内核打补丁。
