# 在openeuler-20.03LTS上安装部署k8s

> # 一、前期准备

- 准备2台（或更多）VM虚拟机，并安装openEuler相同版本
- 其中一台作为master节点，其余作为node节点

> # 二、软件源配置

## 2.1 配置openeuler的yum源（master节点和node节点都需要操作）

```
#打开配置文件
vi /etc/yum.repos.d/

#加入以下内容
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
![图1](https://images.gitee.com/uploads/images/2021/0811/195618_44e89606_9392840.png "1.png")

- 配置完成后，更新yum源

```
yum makecache
```

![图2](https://images.gitee.com/uploads/images/2021/0811/195728_c6561564_9392840.png "2.png")

## 2.2 配置dnf源（master节点和node节点都需要操作）

```
#打开配置文件  
vi /etc/dnf/dnf.conf   
  
#修改为以下内容  
[main]    
gpgcheck=0  
installonly_limit=3  
clean_requirements_on_remove=True  
best=True  
skip_if_unavailable=False  
```

- 主要将`gpgcheck=1`修改为`gpgcheck=0`

![图3](https://images.gitee.com/uploads/images/2021/0811/195941_e715bd9c_9392840.png "3.png")


## 2.3 配置k8s的yum源（master和node节点都需要）

- 由于国内无法访问外网，所以采用阿里云的镜像

```
#执行如下代码，直接配置k8s yum源
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF

#清除缓存
yum clean all

#更新yum源
yum makecache

```

![图4](https://images.gitee.com/uploads/images/2021/0811/200048_4a8ff17d_9392840.png "4.png")


> # 三、安装docker（master和node节点都需要）

## 3.1 docker安装

- k8s集群的创建，需要用到docker、conntrack-tools、socat等工具
- 配置dnf源后，才可以使用dnf安装，直接使用yum安装也是可以的
- 如果安装socat时，报错未找到socat，可以尝试更改yum源（华为云，谷歌云等）

```
dnf install -y docker conntrack-tools socat 
```

## 3.2 查询docker版本

```
docker version
```

![图5](https://images.gitee.com/uploads/images/2021/0811/200150_0163f5b0_9392840.png "5.png")


## 3.3 docker启动失败


![图6](https://images.gitee.com/uploads/images/2021/0811/200246_d49eae79_9392840.png "6.png")


- 若查询docker版本时，发现docker启动失败，可采用以下方法解决：


1. 进入目录`/etc/docker`，发现没有daemon.json文件，可以自己创建一个后，加入如下内容：

```
#注意拼写和英文符号
{
 "registry-mirrors": ["https://registry.docker-cn.com"]
}
```

2. 重启docker

```
systemctl restart docker.service
```

3. 如果还是不成功，则禁用selinux

```
#打开以下文件
vi /etc/sysconfig/selinux

#将SELINUX 改为disabled
```

![图7](https://images.gitee.com/uploads/images/2021/0811/200332_dd800028_9392840.png "7.png")

4. 重启docker

```
systemctl restart docker.service
```

> # 四、k8s安装环境配置（master和node节点都需要）

## 4.1 环境配置

```
# 关闭防火墙  
systemctl disable firewalld  
systemctl stop firewalld  
  
# 关闭selinux  
# 临时禁用selinux  
setenforce 0  
# 永久关闭 修改/etc/sysconfig/selinux文件设置  
vi /etc/sysconfig/selinux  
#修改以下设置  
SELINUX=disabled  
  
# 禁用交换分区  
swapoff -a  
vi /etc/fstab  
#注释掉swap分区  
#/dev/mapper/centos-swap swap  
  
# 修改内核参数  
cat <<EOF >  /etc/sysctl.d/k8s.conf  
net.bridge.bridge-nf-call-ip6tables = 1  
net.bridge.bridge-nf-call-iptables = 1  
EOF
#让上述配置命令生效  
sysctl --system  
  
#或者这样去设置  
echo "1" >/proc/sys/net/bridge/bridge-nf-call-iptables  
echo "1" >/proc/sys/net/bridge/bridge-nf-call-ip6tables  
  
#保证输出的都是1  
cat /proc/sys/net/bridge/bridge-nf-call-ip6tables  
cat /proc/sys/net/bridge/bridge-nf-call-iptables  
```

![图8](https://images.gitee.com/uploads/images/2021/0811/200438_c8cf7cad_9392840.png "8.png")

> # 五、安装k8s（master和node节点都需要）

## 5.1 安装kubelet、kubeadm、kubectl

```
#安装最新版本
yum install -y kubelet kubeadm kubectl  
  
#安装指定版本的kubelet,kubeadm,kubectl  
yum install -y kubelet-1.19.0-0 kubeadm-1.19.0-0 kubectl-1.19.0-0
```

- 若出现报错，无法找到kubelet、kubeadm、kubectl
- 尝试更改yum源后，再重新安装

## 5.2 查看kubelet、kubectl版本

```
kubelet --version
kubeadm version
```

![图9](https://images.gitee.com/uploads/images/2021/0811/200805_89569ecc_9392840.png "9.png")

## 5.3 启动kubelet并设置开机自动启动服务

```
#重新加载配置文件  
systemctl daemon-reload  
  
#启动kubelet  
systemctl start kubelet  
  
#设置开机自启动  
systemctl enable kubelet  
  
#查看kubelet开机启动状态 enabled:开启, disabled:关闭  
systemctl is-enabled kubelet  
```

![图10](https://images.gitee.com/uploads/images/2021/0811/200933_de590220_9392840.png "10.png")

> # 六、初始化k8s集群（只需要master操作）

## 6.1 配置ip_forward

```
#将ip_forward更改为1
echo 1 > /proc/sys/net/ipv4/ip_forward
```

## 6.2 初始化k8s

```
#执行初始化命令  
kubeadm init --image-repository registry.aliyuncs.com/google_containers --apiserver-advertise-address=192.168.0.5 --kubernetes-version=v1.19.3 --pod-network-cidr=10.244.0.0/16 --service-cidr=10.1.0.0/16  
#--apiserver-advertise-address=192.168.83.84 为master的IP  
#--image-repository registry.aliyuncs.com/google_containers为阿里云镜像，国内默认为k8s.gcr.io，需要科学上网 
#--kubernetes-version=v1.19.3为k8s的版本号，与上一步下载的k8s版本号相等，若出现报错信息，可以根据报错信息修改版本号
  
  
#若出现如下报错，则需要再次关闭swap  
#swapoff -a    
[ERROR Swap]: running with swap on is not supported. Please disable swap  
  
  
#打印如下信息，表示创建成功  
W0810 01:14:45.223793   18634 configset.go:348] WARNING: kubeadm cannot validate component configs for API groups [kubelet.config.k8s.io kubeproxy.config.k8s.io]  
[init] Using Kubernetes version: v1.19.3  
[preflight] Running pre-flight checks  
    [WARNING IsDockerSystemdCheck]: detected "cgroupfs" as the Docker cgroup driver. The recommended driver is "systemd". Please follow the guide at https://kubernetes.io/docs/setup/cri/  
    [WARNING Hostname]: hostname "bogon" could not be reached  
    [WARNING Hostname]: hostname "bogon": lookup bogon on 192.168.83.193:53: no such host  
[preflight] Pulling images required for setting up a Kubernetes cluster  
[preflight] This might take a minute or two, depending on the speed of your internet connection  
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'  
[certs] Using certificateDir folder "/etc/kubernetes/pki"  
[certs] Generating "ca" certificate and key  
[certs] Generating "apiserver" certificate and key  
[certs] apiserver serving cert is signed for DNS names [bogon kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.1.0.1 192.168.83.84]  
[certs] Generating "apiserver-kubelet-client" certificate and key  
[certs] Generating "front-proxy-ca" certificate and key  
[certs] Generating "front-proxy-client" certificate and key  
[certs] Generating "etcd/ca" certificate and key  
[certs] Generating "etcd/server" certificate and key  
[certs] etcd/server serving cert is signed for DNS names [bogon localhost] and IPs [192.168.83.84 127.0.0.1 ::1]  
[certs] Generating "etcd/peer" certificate and key  
[certs] etcd/peer serving cert is signed for DNS names [bogon localhost] and IPs [192.168.83.84 127.0.0.1 ::1]  
[certs] Generating "etcd/healthcheck-client" certificate and key  
[certs] Generating "apiserver-etcd-client" certificate and key  
[certs] Generating "sa" key and public key  
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"  
[kubeconfig] Writing "admin.conf" kubeconfig file  
[kubeconfig] Writing "kubelet.conf" kubeconfig file  
[kubeconfig] Writing "controller-manager.conf" kubeconfig file  
[kubeconfig] Writing "scheduler.conf" kubeconfig file  
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"  
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"  
[kubelet-start] Starting the kubelet  
[control-plane] Using manifest folder "/etc/kubernetes/manifests"  
[control-plane] Creating static Pod manifest for "kube-apiserver"  
[control-plane] Creating static Pod manifest for "kube-controller-manager"  
[control-plane] Creating static Pod manifest for "kube-scheduler"  
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"  
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s  
[apiclient] All control plane components are healthy after 20.502340 seconds  
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace  
[kubelet] Creating a ConfigMap "kubelet-config-1.19" in namespace kube-system with the configuration for the kubelets in the cluster  
[upload-certs] Skipping phase. Please see --upload-certs  
[mark-control-plane] Marking the node bogon as control-plane by adding the label "node-role.kubernetes.io/master=''"  
[mark-control-plane] Marking the node bogon as control-plane by adding the taints [node-role.kubernetes.io/master:NoSchedule]  
[bootstrap-token] Using token: uxqtze.cgj4ik3v6zt0j9hf  
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles  
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to get nodes  
[bootstrap-token] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials  
[bootstrap-token] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token  
[bootstrap-token] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster  
[bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace  
[kubelet-finalize] Updating "/etc/kubernetes/kubelet.conf" to point to a rotatable kubelet client certificate and key  
[addons] Applied essential addon: CoreDNS  
[addons] Applied essential addon: kube-proxy  
  
Your Kubernetes control-plane has initialized successfully!  
  
To start using your cluster, you need to run the following as a regular user:  
  
  mkdir -p $HOME/.kube  
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config  
  sudo chown $(id -u):$(id -g) $HOME/.kube/config  
  
You should now deploy a pod network to the cluster.  
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:  
  https://kubernetes.io/docs/concepts/cluster-administration/addons/  
  
Then you can join any number of worker nodes by running the following on each as root:  
  
kubeadm join 192.168.83.84:6443 --token uxqtze.cgj4ik3v6zt0j9hf \  
    --discovery-token-ca-cert-hash sha256:26c592cf0fb5361b4ab85e7826dd23fcf2538a2679ba135d1e27397de54f77d6  
```

- 记住最后的命令`  
kubeadm join 192.168.83.84:6443 --token uxqtze.cgj4ik3v6zt0j9hf \  
    --discovery-token-ca-cert-hash sha256:26c592cf0fb5361b4ab85e7826dd23fcf2538a2679ba135d1e27397de54f77d6`
- 该用于把node节点加入到集群中

## 6.3 按照提示，执行以下代码

```
  mkdir -p $HOME/.kube  
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config  
  sudo chown $(id -u):$(id -g) $HOME/.kube/config  
```

## 6.4 查看k8s集群节点

```
#查看节点  
kubectl get node 
  
#输出  
NAME    STATUS     ROLES    AGE     VERSION  
bogon   NotReady   master   2m27s   v1.19.0  
  
#发现是NotReady状态，原因是没有安装网络插件
#暂时先不管  
```

> # 七、node节点加入集群（只在node节点操作）

## 7.1 预备操作

- 进入node节点的虚拟机，完成步骤一到步骤五

## 7.2 加入集群

```
#如果忘记上一步的命令，在master节点中使用以下代码查询：
kubeadm token create --print-join-command

#获取代码后，在node节点执行
kubeadm join 192.168.1.103:6443 --token mz7qyy.tsdn8hdrp18xrddx  --discovery-token-ca-cert-hash sha256:8523f63f9ff063041f70cae352a32870a9e9998ae265df601d29e668c11dec2f 

#最后在master节点查看加入情况
kubectl get nodes
```

![图11](https://images.gitee.com/uploads/images/2021/0811/201125_7e0026be_9392840.png "11.png")

> # 八、安装flannel（在master操作）

## 8.1 查看日志

- 查询k8s集群情况后，发现都处理NotReady状态，则需要查看日志文件`journalctl -f -u kubelet.service `
- 发现没有flannel网络插件（cni）

```
8月 11 19:39:52 bogon kubelet[3040]: W0811 19:39:52.840747    3040 cni.go:239] Unable to update cni config: no valid networks found in /etc/cni/net.d
8月 11 19:39:53 bogon kubelet[3040]: E0811 19:39:53.984579    3040 pod_workers.go:191] Error syncing pod 88f62889-106a-48f3-897e-9f10be935561 ("kube-flannel-ds-amd64-87qtt_kube-system(88f62889-106a-48f3-897e-9f10be935561)"), skipping: failed to "StartContainer" for "install-cni" with ImagePullBackOff: "Back-off pulling image \"quay-mirror.qiniu.com/coreos/flannel:v0.11.0-amd64\""
8月 11 19:39:54 bogon kubelet[3040]: E0811 19:39:54.888965    3040 kubelet.go:2103] Container runtime network not ready: NetworkReady=false reason:NetworkPluginNotReady message:docker: network plugin is not ready: cni config uninitialized
```

## 8.2 安装flannel

```
#创建文件夹
mkdir flannel && cd flannel

#下载文件
curl -O https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

#国内需要科学上网，才能访问以上网址，我直接在github上下载了fannel配置文件，见该仓库下同一目录的flannel配置文件
#在目录flannel中，复制粘贴该内容

#创建flannel网络插件
kubectl apply -f kube-flannel.yml

#再次查看k8s集群节点，变成Ready状态了
```




 















 

 