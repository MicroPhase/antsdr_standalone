# antsdr-no-OS
Standalone application based on ADI hdl and no_OS for ANTSDR.

## Windows下复原vivado工程

### 所需软件：

- git (用于从github上下载源码)
- vivado2019.1（用于复原工程）
- xilinx sdk2019.1（用于搭建no-OS测试程序）

### 下载源码

首先需要从github上下载对应的源码。打开**git bash**，然后在mingwin中使用如下命令下载源码。

```
git clone -b 2019_r2 --recursive https://github.com/MicroPhase/antsdr_standalone.git
```

![image-20210924190649784](README.assets/image-20210924190649784.png)

注意：在下载源码的时候，使用--recursive会递归的下载子模块当中的文件，只有这样才能保证所需要的版本是一致的。

![image-20221107172649783](README.assets/image-20221107172649783.png)

下载完源码之后，你将会看到有一个**hdl**文件夹。接下来就介绍如何在windows下使用vivado2019.1来复原工程。

### 使用vivado tcl命令行复原工程

关于使用vivado复原工程，可以参考adi官方说明：[ADI HDL Building](https://wiki.analog.com/resources/fpga/docs/build)

打开vivado2019.1，在tcl命令窗口中进入到antsdr工程所在的目录：具体的路径你自己的情况而定。主要是定位到hdl/project/antsdre310或者hdl/project/antsdre200目录下。

![image-20221107172535645](README.assets/image-20221107172535645.png)

然后依次执行如下命令：

```
source ../../scripts/adi_make.tcl
adi_make::lib all
source ./system_project.tcl
```

执行上述命令后，vivado将会依次检查所需要的IP，创建所需要的IP，生成Vivado工程并完成bit文件的生成。

![image-20210924191721108](README.assets/image-20210924191721108.png)

Vivado在构建IP和工程的时候，需要等待较长的时间，请耐心等待。

![image-20210924193419017](README.assets/image-20210924193419017.png)



![image-20210924193351690](README.assets/image-20210924193351690.png)

等到整个工程综合完成之后，可以在该工程的 **antsdre310.sdk**或者**antsdre200.sdk**文件夹下找到硬件描述文件，使用这个硬件描述文件，可以用来搭建no-OS工程。



### 搭建no-OS工程
对于Windows用户，为了简单构建no-OS的过程，请直接使用已经提供好的no-OS源码，也就是在git下载下来的源文件下的app_e310或者app_e200文件夹下的代码。

在前面创建好的vivado工程中，直接Launch SDK.

![image-20210924224958261](README.assets/image-20210924224958261.png)

在SDK当中，新建一个空项目，然后将app文件夹中的代码复制到这个空项目当中。

![image-20210924225207279](README.assets/image-20210924225207279.png)

![image-20210924232231337](README.assets/image-20210924232231337.png)

### 功能测试

接下来就可以连接串口jtag到到电脑上，然后在SDK中生成调试用的elf文件进行调试了。

![image-20210924232424492](README.assets/image-20210924232424492.png)



### NOTE

工程基于ADRV9361,可以支持2R2T。

vivado版本需要使用vivado2019.1,这个版本是Vitis之前的最后一个有SDK的版本。我们许多其他项目也是基于这个版本进行的。(vitis软件太大，后续会看需要升级到vitis)。