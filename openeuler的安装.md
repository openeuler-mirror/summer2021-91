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
![图4](https://images.gitee.com/uploads/images/2021/0811/143756_06ef14e5_9392840.png "4.png")

## 2.3 选择硬件兼容性
![图5](https://images.gitee.com/uploads/images/2021/0811/143806_880eebbc_9392840.png "5.png")

## 2.4 选择稍后安装操作系统
![图6](https://images.gitee.com/uploads/images/2021/0811/143823_5d84f59b_9392840.png "6.png")

## 2.5 选择linux和其他Linux 4.x 64位
![图7](https://images.gitee.com/uploads/images/2021/0811/144026_a14bd4e7_9392840.png "7.png")

## 2.6 修改虚拟机名称和安装位置
![图8](https://images.gitee.com/uploads/images/2021/0811/144039_1b4ab630_9392840.png "8.png")

## 2.7 处理器数量尽量和主机系统相同

- 在win10系统下，可以在任务管理器中，查看处理器个数

![图9](https://images.gitee.com/uploads/images/2021/0811/144051_ed94e502_9392840.png "9.png")

## 2.8 虚拟机内存要选择大于2G（安装k8s时需要）
![图10](https://images.gitee.com/uploads/images/2021/0811/144151_95143257_9392840.png "10.png")

## 2.9 网络类型（桥接网络，NAT都行）
![图11](https://images.gitee.com/uploads/images/2021/0811/144201_f82e1366_9392840.png "11.png")

## 2.10 默认控制器类型
![图12](https://images.gitee.com/uploads/images/2021/0811/144212_d8d5c21b_9392840.png "12.png")
## 2.11 默认磁盘类型
![图13](https://images.gitee.com/uploads/images/2021/0811/144224_e7139575_9392840.png "13.png")

## 2.12 选择创建新磁盘
![图14](https://images.gitee.com/uploads/images/2021/0811/144234_62ac1459_9392840.png "14.png")

## 2.13 选择存储为单个文件，磁盘大小尽量大于15G
![图15](https://images.gitee.com/uploads/images/2021/0811/144247_da9c2d9f_9392840.png "15.png")

## 2.14 指定磁盘文件
![图16](https://images.gitee.com/uploads/images/2021/0811/144257_6c8f53c3_9392840.png "16.png")

## 2.15 最后确认无误后，选择完成
![图17](https://images.gitee.com/uploads/images/2021/0811/144316_27aa4cca_9392840.png "17.png")

## 2.16 在虚拟机设置中，加载第一步下载的镜像
![图18](https://images.gitee.com/uploads/images/2021/0811/144326_313015dc_9392840.png "18.png")

## 2.17 开机后，开始安装openEuler
![图19](https://images.gitee.com/uploads/images/2021/0811/144341_01c20792_9392840.png "19.png")

> # 三、安装openEuler
## 3.1 选择Install openEuler 20.03-LTS
![图20](https://images.gitee.com/uploads/images/2021/0811/144341_01c20792_9392840.png "19.png")

## 3.2 选择安装语言
![图21](https://images.gitee.com/uploads/images/2021/0811/144408_2651e3c1_9392840.png "21.png")

## 3.3 选择安装位置
![图22](https://images.gitee.com/uploads/images/2021/0811/144418_47770d26_9392840.png "22.png")

## 3.4 默认设置就可以了，确定后点击完成
![图23](https://images.gitee.com/uploads/images/2021/0811/144437_638a3e2e_9392840.png "23.png")

## 3.5 设置root密码
- 密码需要三种不同字符
![图24](https://images.gitee.com/uploads/images/2021/0811/144451_7a1333c3_9392840.png "25.png")

## 3.6 等待安装完成，完成后重启机器
![图25](https://images.gitee.com/uploads/images/2021/0811/144507_49a35e2f_9392840.png "26.png")



 