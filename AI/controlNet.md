# 安装和学习。

[toc]

## 电脑环境配置

### 1.安装miniconda

这个是用来管理python版本的，他可以实现python的多版本切换。

下载地址：docs.conda.io/en/latest/miniconda.html

![image-20230325180505250](https://gitee.com/fantastic__cpr/picture/raw/master/img/image-20230325180505250.png)

安装时按默认的一路next就行。

### 2.打开miniconda，输入conda -V 弹出版本号即为正确安装

![image-20230325180730269](https://gitee.com/fantastic__cpr/picture/raw/master/img/image-20230325180730269.png)

![image-20230325180801362](https://gitee.com/fantastic__cpr/picture/raw/master/img/image-20230325180801362.png)

### 3.配置库包下载环境，加快网络速度（替换下载库包地址为国内的清华镜像站）

执行下面

```
conda config --set show_channel_urls yes 
```

生成.condarc 文件

在我的电脑/此电脑-C盘-users-你的账号名下用记事本打开并修改.condarc文件。（如我的路径是C:\Users\859259。）

把下面的内容全部复制进去，全部覆盖原内容，ctrl+s保存，关闭文件。

```
channels: - defaultsshow_channel_urls: truedefault_channels: - 
https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main -
https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r -
https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud 
msys2:https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud 
bioconda:https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud 
menpo:https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud 
pytorch:https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloudpytorch-lts: 
https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud 
simpleitk:https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

运行conda clean -i 清除索引缓存，以确保使用的是镜像站的地址。

**目前有问题**

### 4.创建python环境

建议切换到D盘根目录

### 5.创建python 3.10.6版本的环境

1.自行安装

Install [Python 3.10.6](https://www.python.org/downloads/windows/), checking "Add Python to PATH".

这里我安装到了D:\Python\Python310\路径

[安装教程](https://blog.csdn.net/ruanjimu/article/details/121549510)

2.代码安装

运行下面语句，创建环境。

```
conda create --name stable-diffusion-webui python=3.10.6
```

系统可能会提示y/n, 输入y，按回车即可。

![image-20230325182150035](https://gitee.com/fantastic__cpr/picture/raw/master/img/image-20230325182150035.png)

在你的C:\Users\85925\miniconda3\envs\stable-diffusion-webui已经创建了一个新的项目。

### 6.激活环境

输入conda activate stable-diffusion-webui 回车。

### 7.升级pip，并设置pip的默认库包下载地址为清华镜像。

每一行输入后回车，等执行完再输入下一行，再回车。

```
python -m pip install --upgrade pip
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple(用了最后启动阶段会无限报错)
```

不报错就是完成了。

如果想删除源，用下列命令

```
pip config unset global.index-url
```

或者查看现在用的哪个源

```
pip config list
```

### 8.安装git，用来克隆下载github的项目，比如本作中的stable diffusion webui

前往git官网git-scm.com/download/win

![image-20230326100636462](https://gitee.com/fantastic__cpr/picture/raw/master/img/image-20230326100636462.png)

打开并输入下面指令。

```
git --version
```

查看git的版本，显示版本号即安装成功

### 9.安装cuda

cuda是NVIDIA显卡用来跑算法的依赖程序，所以我们需要它。

打开NVIDIA cuda官网，developer.nvidia.com/cuda-toolkit-archive

回到一开始的miniconda的小窗，输入nvidia-smi，查看你的cuda版本

![image-20230326100834166](https://gitee.com/fantastic__cpr/picture/raw/master/img/image-20230326100834166.png)

下载对应版本

![image-20230326101128350](https://gitee.com/fantastic__cpr/picture/raw/master/img/image-20230326101128350.png)

再命令行窗口通过以下命令查看版本和安装位置

```
nvcc --version
set cuda
```

![image-20230326195312639](https://gitee.com/fantastic__cpr/picture/raw/master/img/image-20230326195312639.png)

好了，完成这步，电脑的基础环境设置终于完事了。

下面开始正式折腾stable diffusion了。

## stable diffusion环境配置

### 1.下载stable diffusion源码

确保你的miniconda里显示的是

```
(stable-diffusion-webui) D:\>
```

输入D:可切换至对应目录下

克隆stable diffusion webui项目

执行下列命令

```
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
```

![image-20230328225545572](https://gitee.com/fantastic__cpr/picture/raw/master/img/image-20230328225545572.png)

显示done即表示完成

注意，现在克隆的本地地址，就是下面经常提到的“项目根目录”。比如，我的项目根目录是D:\stable-diffusion-webui

### 2.下载sd训练模型

打开huggingface.co/CompVis/stable-diffusion-v-1-4-original/tree/main网站

选择sd-v1-4.ckpt

![image-20230328225831924](https://gitee.com/fantastic__cpr/picture/raw/master/img/image-20230328225831924.png)

注：这个模型是用于后续生成AI绘图的绘图元素基础模型库。（下载的很慢啊！！！）

后面如果要用waifuai或者novelai，其实更换模型放进去sd-webui项目的模型文件夹即可。

### 3.模型更名成model.ckpt

下载好之后，请把模型更名成model.ckpt,然后放置在sd-webui的models/stable-diffusion目录下。比如我的路径是D:\stable-diffusion-webui\models\Stable-diffusion

![image-20230329001148591](https://gitee.com/fantastic__cpr/picture/raw/master/img/image-20230329001148591.png)

### 4.启动！！！

在miniconda黑色窗口里，准备开启ai绘画程序，输入

```
cd stable-diffusion-webui
```

![image-20230329001423041](https://gitee.com/fantastic__cpr/picture/raw/master/img/image-20230329001423041.png)

执行

```
webui-user.bat
```

最后这里弄几个小时终于成功了

总结：暂时不要用清华镜像网站去clone，会导致最后启动时无法安装webui模组

清华镜像网站都可以不用配置，科学上网直接一路安装到底就完事了。
