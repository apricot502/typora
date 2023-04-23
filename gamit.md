Ubuntu18.04 安装gamit10.7安装

1、安装依赖环境
1.1 gcc g++ gfortran多版本切换(安装gamit需要切换低版本)
1.1.1安装gcc g++ gfortran多版本

```
	sudo apt-get install gcc-5 gcc-5-multilib g++-5 g++-5-multilib gfortran-5 gfortran-5-multilib
	sudo apt-get install gcc-6 gcc-6-multilib g++-6 g++-6-multilib gfortran-6 gfortran-6-multilib
	sudo apt-get install gcc-7 gcc-7-multilib g++-7 g++-7-multilib gfortran-7 gfortran-7-multilib
```

1.1.2 设置gcc g++ gfortran优先级	

```
	sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-5 70
	sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-6 60
	sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-7 50
	sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-5 70
	sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-6 60
	sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-7 50
	sudo update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-5 70
	sudo update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-6 60
	sudo update-alternatives --install /usr/bin/gfortran gfortran /usr/bin/gfortran-7 50
```

1.1.3 选择gcc和g++ gfortran版本	

```
	sudo update-alternatives --config gcc
	sudo update-alternatives --config g++
	sudo update-alternatives --config gfortran
```

1.2 安装其他依赖包
1.2.1安装csh、tcsh、gfortran、libx11-dev、ncftp、gmt

```
	sudo apt-get install csh tcsh libx11-dev ncftp gmt 
	apt-get install make ##可以不安装
```

1.2.2配置gmt环境
    sudo gedit ~/.bashrc 
	在文档末尾添加如下三行，并保存退出。

```
	# PATH for GMT
	export NETCDFHOME="/usr/lib"
	export GMTHOME="/usr/lib/gmt"
	export PATH="$PATH:$GMTHOME/bin"
```

1.2.3加载修改后的.bashrc 文
	在终端中运行：

```
    source ~/.bashrc
```


3、GAMIT 软件源的准备
	将gamit10.7安装文件复制到 /opt/gamit10.7 文件夹下

4、进入文件夹gamit10.71给 install_software脚本赋执行权限 
	cd /opt/gamit10.71 
	chmod +x install_software 

5、运行安装脚本，开始安装 
    ./install_software
	GAMIT 的安装就会自动开始了。在遇到第一次询问时，直接输入 y 到下一步。遇到第二次询问时，会向你确认X11的路径是否配置正确。这个时候，不要关闭终端，开启另一个终端，用gedit编辑/opt/gamit10.71/libraries目录下Makefile.config文件（注意 Makefile 的大小写）
	sudo gedit /opt/gamit10.71/libraries/Makefile.config
	在打开的 Makefile.config 这个文档中，共有四个地方需要用户手动修改：
5.1 修改 X11 的路径
	需要做的是将文档中 X11 的路径从
    X11LIBPATH /usr/lib/X11
    X11INCPATH /usr/include/X11
	修改为
    X11LIBPATH /usr/lib/x86_64-linux-gnu
    X11INCPATH /usr/include/X11
	
5.2修改 GAMIT 的一些内部参数
	分别是MAXSIT（最大测站数）、MAXSAT（最大卫星颗数）、MAXATM（最大天顶延迟）和MAXEPC（最大历元数）。这里需要改的将MAXSIT改为99，MAXSAT改为40，MAXATM改为25，MAXEPC改为8640。
	修改前：
	MAXSIT 80
	MAXSAT 32
	MAXATM 13
	MAXEPC 2880	
	修改后：
	MAXSIT 99
	MAXSAT 40
	MAXATM 25
	MAXEPC 8640
5.3检查 Linux 操作系统版本号
	Ctrl+F 查找“Linux"
	OS_ID Linux 0001 4930
	另开一个终端，输输入命令查看自己的 Linux 版本： uname -a
	只需记住linux版本的前四位编号，如果小于4930，不需要修改，如果大于4930，则修改为自己的linux版本的前四位编号。笔者的linux版本4.4.0-17763-Microsoft，则编号为4400，小于4930不需修改。	

至此，配置文档里需要手动修改的地方全部修改完毕，保存退出即可。这时候，再回到之前停留在第二次询问的终端窗口中，遇到询问后一路输入 Y 继续安装。不出意外的话，最后就会提示 GLOBK 已经安装成功，并提醒使用者配置路径。

6、配置 GAMIT 环境变量
	在终端中输入：sudo gedit ~/.bashrc
	打开.bashrc 文档后，将以下代码加在在文档末尾：

```
# PATH for GAMIT
export PATH="$PATH:/opt/gamit10.71/gamit/bin:/opt/gamit10.71/com:/opt/gamit10.71/kf/bin"
# export HELP_DIR="/opt/gamit10.7/help/"
```

​	需要注意的是，这里的路径必须是用户自己安装 GAMIT 的路径，不要照搬这里的代码。然后保存退出，在 bash 下加载刚才修改的文件，在终端中运行：
```
source ~/.bashrc
```

7、验证是否安装成功
	验证安装和配置是否成功的方法是在终端内输入 GAMIT/GLOBK 的命令，如果显示命令未找到，则说明在操作中存在错误，请重新安装和配置；如果终端返回该命令的帮助说明，则说明软件已经安装，并配置成功。
	这里给出两个简单的 GAMIT 命令供读者验证：

```
doy
sh_get_rinex
```

​	其中，doy 命令回车后显示帮助文档，则说明 GAMIT 安装成功，环境变量也配置成功。若报错，输入 sh_get_rinex 回车后显示说明文档，则说明 GAMIT 安装成功，但环境变量未配置成功，检查 d）步骤是否操作正确，尤其是符号的输入是否正确。



大气负荷改正模型下载地址为：

```
ftp://everest.mit.edu/pub/GRIDS/
```

在电脑文件夹搜索中打开，下载otl_FES2004.grid。

在gamit10.71中新建GRIDS，见过otl_FES2004.grid复制到GRIDS文件夹中。

