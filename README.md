
# DOL— —从入门到精通
#### 导语：

> The distributed operation layer ([DOL](http://www.tik.ee.ethz.ch/~shapes/dol.html)) is a software development framework to program parallel applications. The DOL allows to specify applications based on the Kahn process network model of computation and features a simulation engine based on SystemC. Moreover, the DOL provides an XML-based specification format to describe the implementation of a parallel application on a multi-processor systems, including binding and mapping.

## 一. DOL框架描述

DOL，即分布式操作层，是用来进行并行式应用编程的一种软件开发架构，它允许应用程序自动地映射到多处理器地平台架构上。
它主要包括三个部分：
    
- **DOL应用程序编程接口**



- **DOL功能仿真**



- **DOL映射转发**




## 二. DOL安装笔记
### 1.环境准备
	（1）C/C++环境配置 
    sudo apt-get update        	# 最好先更新一下源信息
    sudo apt-get install g++
    g++				# 通过g++ 命令验证安装是否成功
    g++: fatal error: no input files
    compilation terminated.

	（2）Java环境配置
	安装jdk-8u40-linux-x64(http://www.oracle.com/technetwork/java/javase/downloads/index.html)
    a. 将安装包解压到 /usr/lib/java
      cd /usr/lib
      sudo mkdir java
     # 进入安装包下载所在路径(在终端打开)
      sudo tar zxvf ./jdk-8u40-linux-x64.gz -C /usr/lib/java
     # 重命名为jdk8
      cd /usr/lib/java
      sudo mv jdk1.8.0_40/ jdk8
    b.配置环境变量
     gedit ~/.bashrc
    # 在打开的文件的末尾添加
    # JDK所在路径
     # enable jdk environment
     export JAVA_HOME=/usr/lib/java/jdk8
     export JRE_HOME=${JAVA_HOME}/jre
     export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
     export PATH=${JAVA_HOME}/bin:$PATH

    # 保存退出，然后输入下面的命令来使之生效
    source ~/.bashrc

    c.配置默认JDK
    # /usr/lib/java/jdk8/为JDK所在路径
    sudo update-alternatives --install /usr/bin/java java /usr/lib/java/jdk8/bin/java 300
    sudo update-alternatives --install /usr/bin/javac javac /usr/lib/java/jdk8/bin/javac 300

    # 查看当前各种JDK版本和配置: 链接组 java (提供 /usr/bin/java)中只有一个候选项：/usr/lib/java/jdk8/bin/java无需配置。
    sudo update-alternatives --config java	# 查看当前各种JDK版本和配置

    d. 通过一下命令验证配置是否成功
    java -version	# 查看JDK版本
    java
    javac

	（3）make和ant环境配置 
	Apache Ant是一个基于Java的构建(Build)工具。理论上讲，类似Unix/Linux C程序员经常使用的Make工具。与Make相比，Ant完全由Java实现，具有跨平台的好处。使用make和makefile工具就可以简洁明快地理顺各个源文件之间纷繁复杂的相互关系。而且如此多的源文件，如果每次都要键入gcc命令进行编译的话，那对程序员来说简直就是一场灾难。而make工具则可自动完成编译工作，并且可以只对程序员在上次编译后修改过的部分进行编译。因此，有效的利用make和makefile工具可以大大提高项目开发的效率
    
	sudo apt-get install ant
    # 使用 ant命令验证安装成功
    ant
    Buildfile: build.xml does not exist!		# 以下是错误信息
    Build failed

### 2.安装和配置SystemC
	解压systemc
   	$	tar -zxvf systemc-2.3.1.tgz
	解压后进入systemc-2.3.1的目录下
	$	cd systemc-2.3.1
	新建一个临时文件夹objdir
	$	mkdir objdir
	进入该文件夹objdir
	$	cd objdir
	运行configure(能根据系统的环境设置一下参数，用于编译)
	$	sudo ../configure CXX=g++ --disable-async-updates
	运行效果如下图所示：
![](http://oe4493xaz.bkt.clouddn.com/%E5%9B%BE%E7%89%871.jpg)

	然后编译
	$	sudo make install
	编译完后文件目录如下($ cd .. $ ls
![](http://oe4493xaz.bkt.clouddn.com/%E5%9B%BE%E7%89%872.jpg)
	记录当前的工作路径(会输出当前所在路径，记下来，待会有用)
	$	pwd
	输出为/root/systemc-2.3.1
	这里表示当前的工作路径为 /root/systemc-2.3.1

### 3.DOL配置

    新建dol的文件夹 
	$	mkdir dol
	将dolethz.zip解压到 dol文件夹中
	$	unzip dol_ethz.zip -d dol

	进入刚刚dol的文件夹
	$	cd ../dol
	修改build_zip.xml文件
	找到下面这段话，就是说上面编译的systemc位置在哪里，
	<property name="systemc.inc" value="YYY/include"/>
	<property name="systemc.lib" value="YYY/lib-linux/libsystemc.a"/>
	把YYY改成上页pwd的结果（注意，对于  64位 系统的机器，lib-linux要改成lib-linux64）

	然后是编译
	$	ant -f build_zip.xml all
	若成功会显示build successful

	接着可以试试运行第一个例子
	进入build/bin/mian路径下
	$	cd build/bin/main
	然后运行第一个例子
	Run example1：
	$ cd build/bin/main
	$ sudo ant -f runexample.xml -Dnumber=1
	成功结果如下图：
![](http://oe4493xaz.bkt.clouddn.com/%E5%9B%BE%E7%89%874.jpg)

至此，DOL的环境就算是配置成功了。


## 三. 实验感想和心得

- 在修改并保存build_zip.xml文件时由于涉及到文件权限问题，需要root账户才有对文件read和write的权限，因此需要以root权限打开文件：
`sudo gedit build_zip.xml `


- 环境配置中遇到第一次build成功，第二次build失败，多半是java环境没有配置好的问题，可以考虑重新配置Java运行环境，或者报错信息中有`Picked up JAVA_TOOL_OPTIONS: -javaagent:/usr/share/java/jayatanaag.jar Unable to locate tools.jar. Expected to find it in /usr/lib/jvm/java-7-openjdk-amd64/lib/tools.jar`的的话一般就是JDK的问题，需要重新安装JDK即可:
`sudo apt-get install openjdk-7-jdk`