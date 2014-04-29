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
提示文本渲染到面板中，然后当用户输入命令时，发送ajax请求到服务端，服务端解析命令文本，并且用相应的工具类来与Linux服务器打交道，把处理结果封装在一个json对象返回到客户端，客户端将结果渲染到面板中。
##开源软件
1. [GateOne](https://github.com/liftoff/GateOne/) 使用10443端口，使用Python，JavaScript，CSS等开发。
2. [ShellInABox](https://code.google.com/p/shellinabox/) 使用4200端口。
3. [WSSH](https://github.com/aluzzardi/wssh/) 使用5000端口，使用Python，JavaScript，CSS开发。
4. [Web-Shell](http://code.google.com/p/web-shell/) 针对iPhone的WebShell。
##安全性分析
1. 通信数据难于过滤。
2. 可以通过上传文件，包括“图片”（仅仅是后缀名为图片类型），压缩文件，数据库文件（可以通过select语句生成WebShell，从而提权）。
