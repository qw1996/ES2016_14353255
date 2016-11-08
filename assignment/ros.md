# Ubuntu下的ROS安装


---
## 1.安装

#### 1.1 配置Ubuntu仓库
配置Ubuntu仓库以允许 "restricted," "universe," 以及"multiverse"等功能。

#### 1.2 安装资源列表
从官网packages.ros.org上下载安装包到计算机安装。

    sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'

#### 1.3 安装密钥
	sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 0xB01FA116

#### 1.4 安装
首先，确认Ubuntu的Debian package index是最新的

	sudo apt-get update
ROS中有很多不同的库和工具，在这里我们选择Desktop-Full Install版本的来安装。

**Desktop-Full Install:** (Recommended) : ROS, rqt, rviz, robot-generic libraries, 2D/3D simulators, navigation and 2D/3D perception

	sudo apt-get install ros-jade-desktop-full
该版本覆盖的功能较为全面，安装的时间也比较长，下载的时候可以去吃饭拉屎。

#### 1.5 初始化rosdep
在使用ROS之前，需要初始化rosdep。因为rosdep可以很容易地为你需要编译的内容安装系统依赖，并且它有一些关键内容需要运行在ROS中。

	sudo rosdep init
	rosdep update

#### 1.6 环境配置
设置ROS的环境变量在每次打开编译器的时候会自动添加到bash session中，从而避免每次都要再去配置。

	echo "source /opt/ros/jade/setup.bash" >> ~/.bashrc
	source ~/.bashrc

#### 1.7 安装ROS工具rosintall
rosintall是ROS的一个常用的命令行工具，它能让你很容易地用一条命令来为ROS包安装许多资源树	。

	sudo apt-get install python-rosinstall

## 2 测试教程
接下来就可以开始测试安装成果了。

#### 2.1 得到安装包的源码
如果你知道每个安装包的代码仓库就可以很容易得到所有源码，但是通常来说这都是很难实现的。现在一个很好的解决方案是仅仅通过apt-get来获取资源，（这里甚至不需要sudo），这一方法可以直接从服务器上下载最新版的安装包资源。

	apt-get source ros-jade-laser-pipeline

也许这里会有一个返回通知告诉你要指定一个具体的，特定的包名。
