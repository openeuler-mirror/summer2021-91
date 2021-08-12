# 在openeuler上安装volcano（没有k8s集群时，通过源代码安装）

> # 一、在openeuler中安装git命令

```
yum -y install git
```

图

> # 二、从github上克隆volcano源码压缩包

```
git clone https://github.com/volcano-sh/volcano.git

#若运行速度太慢，或者克隆失败，可以尝试使用github加速网站
git clone https://gitclone.com/github.com/volcano-sh/volcano.git
```

## 2.1 进入volcano目录

```
cd volcano/

#运行安装脚本
./hack/local-up-volcano.sh
```

图

- 报错：未安装kubctl

> # 三、安装kubectl

## 3.1 下载最新发行版
```
curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
```

图

## 3.2 下载kubectl校验文件（校验文件和文件的版本必须相同）
```
curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"

#验证可执行文件
echo "$(<kubectl.sha256) kubectl" | sha256sum --check

#验证通过时，输出：
kubectl: OK

#验证失败时，输出：
kubectl: FAILED
sha256sum: WARNING: 1 computed checksum did NOT match
```

图

## 3.3 安装kubectl
```
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

## 3.4 查看kubectl版本
```
kubectl version --client
```

> # 四、继续执行安装脚本

## 4.1 报错：没有安装go语言
```
#下载对应版本的安装包
wget https://dl.google.com/go/go1.10.3.linux-amd64.tar.gz

#解压到某个目录（我解压在root下）
tar -zxvf  go1.10.3.linux-amd64.tar.gz

#添加root目录到PATH变量中
vi /etc/profile

#在最后一行添加
export GOROOT=/root/go
export PATH=$PATH:$GOROOT/bin

#最后source一下
source /etc/profile

#查看go语言版本
go version
```

图

## 4.2 报错：没有安装docker

- docker分为（CE(社区免费版)和EE（企业版）），我们需要下载的是docker ce社区版

```
#更新软件源
wget -O /etc/yum.repos.d/openEulerOS.repo https://repo.huaweicloud.com/repository/conf/openeuler_x86_64.repo

#清除缓存
yum clean all

#更新yum源
yum makecache

#yum安装docker
yum install -y docker
```

图

## 4.3 报错：没有安装kind

```
#下载kind二进制安装包  
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.11.1/kind-linux-amd64  
  
#添加执行权限  
chmod +x ./kind  
	  
#移动文件到执行路径  
mv ./kind /usr/local/bin/kind  

#验证kind  
kind version 
```

图

> # 五、继续执行安装脚本

- 又产生报错，目前还正在解决当中

图











 















 

 