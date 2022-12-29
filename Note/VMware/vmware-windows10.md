# 准备工作

1. 获取 [Windows 10镜像](https://www.microsoft.com/software-download/windows10) ，记住镜像文件的位置
2. 获取并安装 [VMWare Workstation 16 PRO](www.vmware.com) （以下简称VM）
3. （可选）准备一个文件夹，用来存放Windows 10虚拟机相关的文件。我的位置是 `C:\Develop\VM\Windows 10` 



# 实际操作

## 配置Windows 10虚拟机

### 开始创建

打开VM，点击 `Create a New Virtual Machine` ，选择 `Custom` ，点击 `Next` 

![01.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\01.png)



### 虚拟机特性版本

默认即可，点击 `Next` 

![02.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\02.png)



### 系统安装方式

选择第3个选项，等配置完成后再指定安装方式，点击 `Next` 

![03.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\03.png)



### 选择要安装哪种系统

默认就是Windows，点击 `Next` 

![04.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\04.png)



### 命名虚拟机，定义安装位置

以后我们可能会安装另一个虚拟机，起名以便区分，还可自定义虚拟机的安装位置。 `修改完成后` 或者 `直接` 点击 `Next` 

- 我给它取名为 `tina-VM Windows 10` ，存放在`C:\Develop\VM\Windows 10` 

![05.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\05.png)



### 系统引导方式 <span id="yindaofangshi"> </span> 

两种都可以， `BIOS` 引导在安装系统时步骤简洁。这里使用 `UEFI` 进行引导，以便让你知道怎么处理这种引导方式在安装系统时的问题。点击 `Next` 

![06.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\06.png)



### 分配处理器核心和线程 <span id="hexinxiancheng"> </span> 

以我的电脑为例，CPU是6核12线程，我选择分配1个核心和6条线程给虚拟机。点击 `Next` 

- 无论怎样分配，核心数（ `Number of processors` ）我们都要选 `1` ，否则会出现虚拟机实际得到的核心和线程与预期不符的问题
  - 例如，我计划分配2个核心，3条线程，在此处 **务必** 写成1个核心，6个线程
    - 如果写成2个核心，3条线程，则实际分配到的核心与线程的乘积将小于6，导致虚拟机运行卡顿

- 分配多少核心和线程给虚拟机，应当是自身情况而定， **不能照抄** 

![07.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\07.png)



### 分配内存

这里我选择分配8G，点击 `Next` 

- 分配多少内存给虚拟机应当是自身情况而定， **不能照抄** 

![08.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\08.png)



### 网络、虚拟硬盘类型等设置

接下来的6步使用默认设置即可

![09.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\09.png)



![10.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\10.png)



![11.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\11.png)



![12.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\12.png)



![13.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\13.png)



![14.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\14.png)



### 完成配置

点击 `Finish` 

![15.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\15.png)



### 指定系统安装方式

> 第三步中，我们没有指定系统安装方式，此处进行指定。

点击 `Browse...` 选择Windows 10镜像文件。点击 `OK` 

![16.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\16.png)



## 安装Windows 10

上面的步骤完成后，如果你看到的是这个界面，且你在 [系统引导方式](#yindaofangshi)  **选择了BIOS引导** ，那就可以点击 `Power on this virtual machine` 开始安装了， **该部分可以不看** 

![17.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\17.png)



### UEFI引导导致的问题与解决

如果我们在 [系统引导方式](#yindaofangshi) 选择了UEFI引导，点击 `Power on this virtual machine` 后会出现以下画面，提示 `Time out.` 。我们无需操作，等待即可。

![18.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\18.png)

片刻之后，会出现一个蓝色的框框，鼠标点击蓝色框的任一部分，移动光标至 `EFI UMware Uirtual SATA CDROM Drive (1.0)` ，然后回车。此时就会进入Windows 10安装界面

![19.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\19.png)



# 问题

## 卡在”准备就绪“

系统安装完后，卡在“准备就绪”，一直转圈圈。这可能是因为你在 [分配处理器核心和线程](#hexinxiancheng) 时填写错误，导致虚拟机没有分配到你设想的核心数和线程数。

- 还有一种可能就是你的物理机硬件性能较差，不在本方法讨论范围内。

![20.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\20.png)

### 解决办法

方法一：等待，但可能会长达半个多小时，受你的硬件性能等因素影响

方法二（推荐）：关闭虚拟机，点击 `Edit virtual machine settings` -> `Prosessors` 进行 [修改](#hexinxiancheng) ，再开机即可

![21.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\21.png)

![22.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\22.png)



## 系统画面过小

虚拟机画面没有填满屏幕

![23.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\23.png)

### 解决办法

安装 `VMware tools` 即可，点击 `VM` -> `Install VMware Tools...` ，在虚拟机中打开资源管理器，点击 `DVD驱动器` ，选择 `典型安装` ，按照提示安装即可

![24.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\24.png)

![25.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\25.png)

![26.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\26.png)

### 效果

![27.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\27.png)



## 某些画面发虚发白

这个常见于浏览器的二级菜单，如下图红圈

![28.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\28.png)

![29.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\29.png)

### 解决办法

关闭虚拟机，点击 `Edit virtual machine settings` -> `Display` ，关闭 `3D graphics` 后，重启即可

![30.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\30.png)

![31.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\31.png)

### 效果

![32.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\32.png)

还有一种办法，只对Chrome生效，就是在Chrome的设置里，关闭 `系统` 中的 `硬件加速` ，也能解决这个问题。了解即可



## 为虚拟机C盘扩容

使用一段时间后，我们可能会产生为虚拟机C盘扩容的想法。

### 解决办法

第一步：关闭虚拟机，点击 `Edit virtual machine settings` -> `Hard Disk` -> `Expand` ，填写扩容后的总容量（务必要小于物理磁盘的剩余容量）。

![33.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\33.png)

![34.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\34.png)

第二步：开启虚拟机，在开始菜单上右键，找到磁盘管理。如果C盘旁边就是未分配的空间，则在C盘上右键，点击扩展卷即可。 <span id="dierbu"> </span> 

![35.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\35.png)



如果C盘旁边有其他分区（通常是恢复分区），则需要先将其删除，才能进行第二步。

![36.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\36.png)

第三步：打开cmd，输入 `diskpart` ，回车

第四步：输入 `lisk disk` ，回车，得到以下输出

![37.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\37.png)

第五步：输入 `select disk 0` ，回车

第六步：输入 `list partition` ，回车，得到以下输出

![38.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\38.png)

第七步：输入 `select partition 4` ，回车

第八步：输入 `delete partition override` ，回车，得到以下输出则表示分区删除成功

![39.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\39.png)

此时再执行 [第二步](#dierbu) 即可完成扩容



## 设置共享文件夹

VMWare为我们提供了共享文件夹的功能，帮助我们在物理机和虚拟机之间快速传输文件。

第一步：关闭虚拟机，点击点击 `Edit virtual machine settings` -> `Options` -> `Shared Folders` ，选择 `Always enabled` ，点击 `Add...` 

![40.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\40.png)

第二步：设定要共享的文件夹的路径，并为它起一个方便记忆的名字（这个名字就是该文件夹在虚拟机资源管理器中的名字）

![41.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\41.png)

第三步：打开虚拟机，打开资源管理器，点击 `此电脑` ，点击 顶部的`计算机` -> `映射网络驱动器` 

![42.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\42.png)

第四步：将 `\\vmware-host\Shared Folders\你的共享文件夹的名字` 粘贴在文件夹处，点击 `完成` 即可

![43.png](D:\Files\github\jamho-kong.github.io\docs\images\VM\VM-win10\43.png)