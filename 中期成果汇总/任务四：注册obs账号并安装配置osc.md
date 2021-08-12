# 注册OBS账号，并安装配置OSC

> # 一、注册账号

- 打开网址`https://build.openeuler.org/user/signup`，填写相关信息后，选择`sign up`

![图1](https://images.gitee.com/uploads/images/2021/0812/161139_58f775af_9392840.png "1.png")

- 进入如下界面

![图2](https://images.gitee.com/uploads/images/2021/0812/161200_8e69b43c_9392840.png "2.png")

> # 二、OSC安装配置

- osc是obs的客户端，提供用于跟obs交互的API命令行，可以很方便的在本地跟OBS Server进行交互，几乎所有页面上操作都有对应的osc命令行。

## 2.1 配置openeuler的yum源（可以参考任务二的过程，也可以直接配置如下yum源）

```
#打开配置文件
vi /etc/yum.repos.d/openEuler_x86_64.repo

#加入以下内容
[openEuler]
name=openEuler_20.03_LTS_everything
baseurl=https://repo.openEuler.org/openEuler-20.03-LTS/everything/×86_64/
enabled=1
gpgcheck=1
gpgkey=https://repo.openEuler.org/openEuler-20.03-LTS/everything/×86_64/RPM-GPG-KEY-openEuler
```

- 配置完成后，更新yum源
```
yum makecache
```

## 2.2 yum命令安装osc软件包

```
# -y 表示全部默认yes
yum install osc -y
```
![图3](https://images.gitee.com/uploads/images/2021/0812/161225_2b25fa46_9392840.png "3.png")
![图4](https://images.gitee.com/uploads/images/2021/0812/161236_55ccb027_9392840.png "4.png")

## 2.3 osc的配置

- osc配置文件的地址为`/root/.oscrc`
- 若没有该文件，可以自己创建
- 注意修改obs的名称和密码
```
#打开该文件
vi /root/.oscrc

#加入以下内容
[general]
apiurl = https://bulid.openeuler.org
no_verify = 1

[https://bulid.openeuler.org]
user=”your obs name”
pass=”your password”

#查看文件内容
nl /root/.oscrc
```

![图5](https://images.gitee.com/uploads/images/2021/0812/161349_f2383c19_9392840.png "5.png")









 















 

 