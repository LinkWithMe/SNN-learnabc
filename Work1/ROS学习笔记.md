# ROS学习笔记

[TOC]

## 1.安装

### 1.1 添加ROS软件源

```
sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
```

### 1.2 添加密钥

```
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```

添加成功后的结果：

![1660884436245](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1660884436245.png)

### 1.3 更新软件源

```
sudo apt update
```

更新后的结果：

![1660884501344](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1660884501344.png)

### 1.4 安装

```
sudo apt install ros-noetic-desktop-full
```

安装完成后的结果：

![1660887352351](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1660887352351.png)

## 2.配置与安装检查

### 2.1 初始化rosdep

输入初始化命令：

```
sudo rosdep init
```

出现错误：

![1660893169752](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1660893169752.png)

尝试命令：

```
sudo apt install python3-rosdep2
```

安装成功后再次进行初始化，出现错误：

![1660893249580](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1660893249580.png)

输入命令：

```
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc 
```

再次进行初始化，成功：

![1660893352503](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1660893352503.png)

### 2.2 配置环境变量

输入以下命令：

```
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc 

source ~/.bashrc 
```

### 2.3 安装rosinstall

```
sudo apt install python3-rosinstall python3-rosinstall-generator python3-wstool
```

安装完成后结果：

![1660893707637](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1660893707637.png)

### 2.4 验证ROS是否安装成功

输入命令：

```
roscore
```

出现提示信息：

![1660893784752](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1660893784752.png)

执行以下命令：

```
sudo apt install python3-roslaunch
```

再次运行，结果如下：

![1660893910132](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1660893910132.png)

错误原因可能是因为之前的安装没有安装全，执行：

```
sudo apt install ros-noetic-desktop-full
```

再次执行roscore，结果如下：

![1660894002308](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1660894002308.png)

安装完毕

## 3.学习笔记

### 3.1 用vim编写代码

首先：

```
vim CMakeLists.txt
```

修改部分：

![1661963581772](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1661963581772.png)

①放开注释

②更改文件名称

最终结果：

![1661963795561](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1661963795561.png)

### 3.2 话题通信的关注点

![1662087465165](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1662087465165.png)

## 4.遇到的错误以及解决

### 4.1 无法连接到qt

运行后出现错误如下：

![1661930285010](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1661930285010.png)

将DISPLAY定位到自己电脑端口：

```
export DISPLAY=10.61.88.210:0.0
```

再次运行之后，无报错，但是无窗口：

![1661930927493](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1661930927493.png)

尝试运行键盘操作界面：

![1661930943103](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1661930943103.png)

可以正常运行，但是就是没有窗口界面

参考了以下文章：

![1661932731252](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1661932731252.png)

发现是因为自己没有**GUI**的问题，看到文章所写的提到，目前WSL2已经可以直接安装Linux的GUI，于是赶紧下载：

```
sudo apt update

sudo apt install gedit -y

sudo apt install gimp -y

sudo apt install nautilus -y

sudo apt install vlc -y

sudo apt install x11-apps -y
```

再次运行：

![1661932986012](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1661932986012.png)

成功！

### 4.2 catkin_make出现错误

报错：

![1661938042949](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1661938042949.png)

执行命令：

```
pip install empy
```

报错为：

![1661938343980](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1661938343980.png)

执行命令：

```
pip install -U rosdep rosinstall_generator wstool rosinstall six vcstools

conda install empy
```

再次执行，成功：

![1661962453259](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1661962453259.png)

### 4.3 Linux无法安装vs code安装包

将安装包转移至linux中，执行安装出现如下错误：

![1662007480163](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1662007480163.png)

执行命令：

```
sudo dpkg -i code_1.70.2-1660629410_amd64.deb
```

安装成功

### 4.4 无法打开vs code

报错信息：

![1662008372692](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1662008372692.png)

输入了两条命令：

```
sudo code /directory-to-open --user-data-dir='.' --no-sandbox
```

成功打开：

![1662008877019](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1662008877019.png)

### 4.5 vscode中编译ros失败

在配置中使用如下代码：

```c
{
// 有关 tasks.json 格式的文档，请参见
    // https://go.microsoft.com/fwlink/?LinkId=733558
    "version": "2.0.0",
    "tasks": [
        {
            "label": "catkin_make:debug", //代表提示的描述性信息
            "type": "shell",  //可以选择shell或者process,如果是shell代码是在shell里面运行一个命令，如果是process代表作为一个进程来运行
            "command": "catkin_make",//这个是我们需要运行的命令
            "args": [],//如果需要在命令后面加一些后缀，可以写在这里，比如-DCATKIN_WHITELIST_PACKAGES=“pac1;pac2”
            "group": {"kind":"build","isDefault":true},
            "presentation": {
                "reveal": "always"//可选always或者silence，代表是否输出信息
            },
            "problemMatcher": "$msCompile"
        }
    ]
}
```

修改完文件之后，再次编译报错：

![1662010924084](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1662010924084.png)

尝试过多种方法，但是没有找到解决之道，但是：

我在

- vs code中进行代码编写
- 命令行进行代码编译和运行

结果成功：

![1662033696172](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1662033696172.png)

- vs code中进行代码编写
- vs code中不使用视频给的代码，默认配置代码，然后在vs code上运行：

![1662033820382](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1662033820382.png)

依旧成功

？

需要注意，

```
rosrun helloworld_vs helloworld_vs
```

其中两个名称分别是**修改的两处的映射的文件名**

### 4.6 vscode中出现RLException: Invalid roslaunch XML syntax

执行命令：

```
roslaunch helloworld_vs start_tu.launch
```

报错信息：

![1662048299829](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1662048299829.png)

改正方法：

写完launch文件后，将launch文件**保存后再执行**。

### 4.7 服务通信模式下，客户端请求时，服务端Segmentation fault导致通信失败

在学习过程中，出现以下错误：

![1662276733923](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1662276733923.png)

显示：**服务器端可以启动，但是在响应请求时，会报错**

百度查询的可能出错原因：

- 空指针访问
- 指针指向非法区域后的写操作
- 常量空间破坏
- 非对齐访问也可能导致 

受到启发，是否是因为数据的格式出错，修改之后果然：

修改之前：

```c++
ROS_INFO("result:%s",sum);
```

修改之后：

```c++
ROS_INFO("result:%d",sum);
```

成功执行：

![1662277100286](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1662277100286.png)

## 5.常用命令

### 5.1 将windows文件转移至WSL

```
mv /mnt/windows下源文件 WSL文件位置
mv /mnt/d/mydataset/code_1.70.2-1660629410_amd64.deb /root/download
```

结果：

![1662007035606](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1662007035606.png)

## 6.未解决的问题

### 6.1 图形界面的问题，使用的图形界面在关闭时有时会卡死

学习中，因为本身自带的一些GUI图形功能有问题，于是查阅相关资料 [使用 WSL 运行 Linux GUI 应用 | Microsoft Docs](https://docs.microsoft.com/zh-cn/windows/wsl/tutorials/gui-apps) 

下载了适用于 Linux 的 Windows 子系统的GUI应用，最终解决了问题

**但是在使用过程中出现了问题:**

- 小概率，在wsl没有关闭时(即使没有打开任何端口)，会对电脑的正常使用造成影响，如应用程序闪烁以及掉帧
- 中等概率，开启的图形窗口，在关闭时可能会卡住，同时鼠标右键无反应

所以，在学习过程中，调用了很多次：

```
wsl --shutdown
```

可以解决问题，但是不知道**这样一次性关闭虚拟机，是否有坏处？**

### 6.2 启动vs code时必须使用其他指令，vs code项目中有很多文件

在启动vs code时，必须使用如下命令才能成功开启：

```
sudo code /directory-to-open --user-data-dir='.' --no-sandbox
```

且在开启后，打开具体的文件夹，会有很多很多文件，并不是像视频中那样只有几个文件：

![1662047545568](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1662047545568.png)

感觉这么多文件可能与执行的那一行指令有关，**但不知道这么多文件会对后续使用产生什么影响？**

### 6.3 快捷键 ctrl + shift + B 调用编译，修改.vscode/tasks.json 文件有什么用?

在vscode中编译ros时，需要这一步：

![1662034183198](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1662034183198.png)

但是我执行之后，一直报错：

![1662010924084](C:\Users\17799\AppData\Roaming\Typora\typora-user-images\1662010924084.png)

改了好久没改好...

但是之前不用vscode是可以运行的，而且现在即使没配置好，vs也可以进行写代码，那问题?

于是我：

- vs code中进行代码编写
- 命令行进行代码编译和运行

结果成功

- vs code中进行代码编写
- vs code中不使用视频给的代码，默认配置代码，然后在vs code上运行

结果依旧成功，也就是说，我用**默认的 .vscode/tasks.json  配置文件**也成功运行了

不知道改这里的文件有什么埋伏

## 参考资料

[1] [详细介绍如何在ubuntu20.04中安装ROS系统，超快完成安装（最新版教程）_石头1666的博客-CSDN博客_ubuntu20.04安装ros](https://blog.csdn.net/u014541881/article/details/125050643) 

[2] [解决 qt.qpa.xcb: could not connect to display 问题_hypc9709的博客-CSDN博客_qt.qpa.xcb](https://blog.csdn.net/hypc9709/article/details/124238176) 

[3] [使用 WSL 运行 Linux GUI 应用 | Microsoft Docs](https://docs.microsoft.com/zh-cn/windows/wsl/tutorials/gui-apps) 