# openeuler的yum源配置和常用软件包的下载

> # 一、配置yum源

- 配置yum源需要复制粘贴代码，所以使用Xshell

## 1.1 查看虚拟机ip地址

```
ip addr
```

![图1](https://images.gitee.com/uploads/images/2021/0811/160703_506484d3_9392840.png "y1.png")

- 如果发现没有inet地址（ipv4），或者ens33网卡丢失

![图2](https://images.gitee.com/uploads/images/2021/0811/160912_e27deba1_9392840.png "y2.png")

- <strong>解决方法如下</strong>：

1. 查看配置文件`/etc/sysconfig/network-scripts/ifcfg-ens33`是否还在

```
cd /etc/sysconfig/network-scripts/
ls
```
![图3](https://images.gitee.com/uploads/images/2021/0811/160843_fa1b5082_9392840.png "3.png")

2. <strong>若配置文件还在</strong>，修改配置文件`OBBOOT=yes`

![图4](https://images.gitee.com/uploads/images/2021/0811/161021_5353cf4b_9392840.png "4.png")

3. 顺序执行以下代码，关闭网络服务，重新启动

```
service NetworkManager stop 
service NetworkManager start
```

4. 重新查看`ip addr`

## 1.2 登录Xshell

- 打开Xshell，新建会话，输入ip地址后点击连接

![图5](https://images.gitee.com/uploads/images/2021/0811/161137_b0bf3c82_9392840.png "5.png")

## 1.3 配置yum源

- 国内有些网站无法正常访问，所以需要配置华为镜像，具体内容见下方代码

- 只配置[base]yum源，可以造成部分软件包无法安装的情况，所以我在repo文件中加入了以下所有内容

```
#打开repo文件
vi /etc/yum.repos.d/openEuler_x86_64.repo

#复制以下内容到repo文件中
[base]
name=openEuler20.03LTS
baseurl=https://repo.openeuler.org/openEuler-20.03-LTS/OS/x86_64/
enabled=1
gpgcheck=0

[repository]
name=openEuler
baseurl=https://mirrors.huaweicloud.com/openeuler/openEuler-20.03-LTS/OS/x86_64/

[openEuler-source]
name=openEuler-source
baseurl=https://repo.huaweicloud.com/openeuler/openEuler-20.03-LTS/source/
enabled=1
gpgcheck=1
gpgkey=https://repo.huaweicloud.com/openeuler/openEuler-20.03-LTS/source/RPM-GPG-KEY-openEuler

[openEuler-os]
name=openEuler-os
baseurl=https://repo.huaweicloud.com/openeuler/openEuler-20.03-LTS/OS/x86_64/
enabled=1
gpgcheck=1
gpgkey=https://repo.huaweicloud.com/openeuler/openEuler-20.03-LTS/OS/x86_64/RPM-GPG-KEY-openEuler

[openEuler-everything]
name=openEuler-everything
baseurl=https://repo.huaweicloud.com/openeuler/openEuler-20.03-LTS/everything/x86_64/
enabled=1
gpgcheck=1
gpgkey=https://repo.huaweicloud.com/openeuler/openEuler-20.03-LTS/everything/x86_64/RPM-GPG-KEY-openEuler

[openEuler-EPOL]
name=openEuler-epol
baseurl=https://repo.huaweicloud.com/openeuler/openEuler-20.03-LTS/EPOL/x86_64/
enabled=1
gpgcheck=0
```

- 更新yum源

```
yum makecache
```

![图6](https://images.gitee.com/uploads/images/2021/0811/161302_f0a4e56e_9392840.png "6.png")

- 查询已经成功配置的yum源

```
yum repolist
```

![图7](https://images.gitee.com/uploads/images/2021/0811/161351_c65fc876_9392840.png "7.png")


> # 二、安装几个常用的包

```
yum -y install net-tools git tar lrzsz
```

- net-tools 可以使用ifconfig 查看虚拟机的网络配置
- lrzsz 是Xshell与主机交换文件的命令包
- tar 与压缩包相关
- git 是一个开源的分布式版本控制系统，用git相关命令管理自己的文件或下载文件

 

 