># 在VMWare Workstation15 pro 上，安装OpenEuler-20.03-LTS

> # 一、下载OpenEuler镜像
## 1.1 在以下链接中选择x86_64/

- `https://repo.openeuler.org/openEuler-20.03-LTS/ISO/`

![图1](https://images.gitee.com/uploads/images/2021/0811/143606_8c84ce54_9392840.png "1.png")

## 1.2 下载以下镜像（若浏览器下载速度慢，可以保存下载链接后，使用迅雷下载）

![图2](https://images.gitee.com/uploads/images/2021/0811/143643_8009e0f5_9392840.png "2.png")

## 1.3 下载完成后，开始创建虚拟机


> # 二、创建VM虚拟机
## 2.1 打开VMare****软件
![图3](https://images.gitee.com/uploads/images/2021/0811/143656_06c398b6_9392840.png "3.png")

## 2.2 选择自定义安装
![图4](https://images.gitee.com/uploads/images/2021/0811/143656_06c398b6_9392840.png "4.png")

## 2.3 选择硬件兼容性
![图5](https://images.gitee.com/uploads/images/2021/0811/143656_06c398b6_9392840.png "5.png")

## 2.4 选择稍后安装操作系统
![图6](https://i.loli.net/2021/08/11/wnyYXoxdhRv8Cz2.png)

## 2.5 选择linux和其他Linux 4.x 64位
![图7](https://i.loli.net/2021/08/11/j318apm5JCAkXDe.png)

## 2.6 修改虚拟机名称和安装位置
![图8](https://i.loli.net/2021/08/11/qFRNcEhQ3CGpW5S.png)

## 2.7 处理器数量尽量和主机系统相同
- 在win10系统下，可以在任务管理器中，查看处理器个数
![图9](https://i.loli.net/2021/08/11/SAJtgWKq9oj13bI.png)

## 2.8 虚拟机内存要选择大于2G（安装k8s时需要）
![图10.png](https://i.loli.net/2021/08/11/LRVIxcbKQvWnlJ2.png)

## 2.9 网络类型（桥接网络，NAT都行）
![图11](https://i.loli.net/2021/08/11/T5hDaG3UcAHLole.png)

## 2.10 默认控制器类型
![图12.png](https://i.loli.net/2021/08/11/zdU1XPywYpvWjrl.png)

## 2.11 默认磁盘类型
![图13](https://i.loli.net/2021/08/11/j28LaKB9WO3RAXg.png)

## 2.12 选择创建新磁盘
![图14](https://i.loli.net/2021/08/11/k8F7dHABqSfV9l1.png)

## 2.13 选择存储为单个文件，磁盘大小尽量大于15G
![图15](https://i.loli.net/2021/08/11/neo9ImHANucg1Lw.png)

## 2.14 指定磁盘文件
![图16](https://i.loli.net/2021/08/11/Ed7pglv1C92mRZT.png)

## 2.15 最后确认无误后，选择完成
![图17](https://i.loli.net/2021/08/11/RL4TuKrGfX2vkIw.png)

## 2.16 在虚拟机设置中，加载第一步下载的镜像
![图18](https://i.loli.net/2021/08/11/86umQqC7VJfsG31.png)

## 2.17 开机后，开始安装openEuler
![图19](https://i.loli.net/2021/08/11/vTROutypWkC5Kz3.png)

> # 三、安装openEuler
## 3.1 选择Install openEuler 20.03-LTS
![图20](https://i.loli.net/2021/08/11/vTROutypWkC5Kz3.png)

## 3.2 选择安装语言
![图21](https://bj.bcebos.com/shitu-query-bj/2021-08-11/14/e3a74a971a0604e6?authorization=bce-auth-v1%2F7e22d8caf5af46cc9310f1e3021709f3%2F2021-08-11T06%3A18%3A02Z%2F300%2F%2Feec0fb318ff56e53c9bca27c69e6dbfe62fb68559abd25c9342d4e7c80f1cca0)

## 3.3 选择安装位置
![图22](https://bj.bcebos.com/shitu-query-bj/2021-08-11/14/44d463a3169f9eea?authorization=bce-auth-v1%2F7e22d8caf5af46cc9310f1e3021709f3%2F2021-08-11T06%3A20%3A10Z%2F300%2F%2Fb14d22ae674d1eb71b3d053cfb56362bab5545591a29c772d199c970b1d530c5)

## 3.4 默认设置就可以了，确定后点击完成
![图23](https://bj.bcebos.com/shitu-query-bj/2021-08-11/14/3c8379916f50226b?authorization=bce-auth-v1%2F7e22d8caf5af46cc9310f1e3021709f3%2F2021-08-11T06%3A22%3A10Z%2F300%2F%2F33d9ebb91e49108d43b16b30d3d3d104267977f5da66897f46fcd4521e4b2434)

## 3.5 设置root密码
- 密码需要三种不同字符
![图24](https://bj.bcebos.com/shitu-query-bj/2021-08-11/14/579b9a98e1bc2bcb?authorization=bce-auth-v1%2F7e22d8caf5af46cc9310f1e3021709f3%2F2021-08-11T06%3A23%3A30Z%2F300%2F%2F4bbe31c81eaafaeb4711cd44a17f36504a2f91f80c6bc724a1387d0e2df397dc)

## 3.6 等待安装完成，完成后重启机器
![图25](https://bj.bcebos.com/shitu-query-bj/2021-08-11/14/d413ef85d6a34651?authorization=bce-auth-v1%2F7e22d8caf5af46cc9310f1e3021709f3%2F2021-08-11T06%3A24%3A54Z%2F300%2F%2F6a12a10616f024bc1dd419e236a1db4d6c71d2a90b6ee4e46df7c1a98736ec8c)



 