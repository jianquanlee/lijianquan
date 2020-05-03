&emsp;&emsp;正所谓，工欲善其事必先利其器，一个好的工具能够极大地提高工作效率，今天就来介绍一下**Linux环境管理神器-Anaconda**。

&emsp;&emsp;也是一样，首先来说你为什么需要它。老师布置了个任务，你赶忙在网上找代码，欣喜地发现GitHub上有类似的代码，赶忙下载下来，下载来了发现，这货用的Python 2.7，而你系统上装的是Python 3.6，不能用啊，不过你一想，先跑通了再说，于是卸载了系统上的Python 3.6，装上Python 2.7，寄希望于下载来的代码能赐予你神秘的魔力，很可惜，这个代码Work不了，你试了很久也没办法，只能再次去GitHub上寻找精神支柱。突然你又发现了荒漠中的泉水，找到了另一份代码，但是呢，这份代码是用Python 3.6写的，你恨自己刚卸载了这个版本的Python，没办法，再重新来一遍呗，这时，你突然想起曾经一个高人跟你说过一个工具，叫**Anaconda**，当时你瞧不上，表示打死也不用。但是万事逃不过真香定律，你现在又回来百度了一波，然后凑巧发现了这篇文章。

&emsp;&emsp;言归正传，Anaconda是一个Linux下的环境管理神器，解决的就是上面的问题，不同的代码需要不同的环境，避免了重复环境装卸的问题。网上关于Anaconda的博客也算是挺多的，但是吧，都不齐活，我根据自己的学习经验，将相关内容进行归纳概括。
#### Anaconda官网
&emsp;&emsp;首先放上[Anaconda官网](https://www.anaconda.com/)，毕竟官网的内容是最靠谱的，更新也是最及时的，拿捏不准的东西看看官网，`https://www.anaconda.com/`。
#### Anaconda下载及安装
&emsp;&emsp;在官网提供了下载链接，用户可以根据自己的系统进行下载安装。就在`https://www.anaconda.com/products/individual`[这个页面](https://www.anaconda.com/products/individual)的最下方，然后上传到服务器端，利用`sh`命令就可以安装，当然也可以利用`wget`命令进行安装包下载。这里要说另一个软件，**Miniconda**，这个软件差不多算是Anaconda的瘦身版吧，对于一般用户而言，两者没有太大的差别，在使用上也是基本类似的，我一般就用Miniconda。`https://docs.conda.io/en/latest/miniconda.html`，这个呢，就是Miniconda的网页。

&emsp;&emsp;在安装完成的时候，会问你是否要将启动指令写入到.bashrc里面，一般都会选择`yes`，要是`no`的话，还得自己再进行配置，比较麻烦。
#### Anaconda的基本使用
&emsp;&emsp;安装完Anaconda之后，就可以进行使用了，下面介绍一些基本操作和指令。关于Conda的所有内容，[官网上](https://docs.conda.io/projects/conda/en/latest/commands.html)都有，而且非常详细，这里只是给大家做一个参考。

&emsp;&emsp;所谓授人以鱼不如授人以渔，下面这一条最重要。

```python
# 查询conda命令
conda -h
conda --help
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429161431892.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xvdmVfbGpx,size_16,color_FFFFFF,t_70)
&emsp;&emsp;通过这个指令可以看到所有的conda指令，比如有`conda create`这个指令，然后继续输入`conda create -h`就可以看到可以跟在后面的参数了。这里的话，`-h`和`--help`作用是一样的。

&emsp;&emsp;首先来看一下Conda的版本号，这个指令就是：
```python
# 查看当前conda版本
conda --version
conda -V
```
&emsp;&emsp;再查看一下当前有啥conda环境。
```python
# 查看所有的conda环境
conda env list
conda info --envs
```
&emsp;&emsp;在没有自行安装其他的conda环境的时候，默认会有一个`base`环境，也就是输入以上命令，会出现以下的界面。上面两个指令的含义是一样的，只要记住一个就行了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429161921312.png#pic_center)
&emsp;&emsp;这里的`*`表示，这个是当前处于激活状态的环境。每次进入conda的时候，默认就是进入了base环境。
```python
# 新建conda环境
conda create -n enviroment
```
&emsp;&emsp;只有一个环境，那肯定不行啊，所以就利用上面的指令新建环境。比如`conda create -n pytorch`就是建立了一个名为`pytorch`的环境。建完环境之后，再输入`conda env list`就可以看到刚才建的那个环境了，那怎么进到这个环境里面呢。
```python
# 进入conda环境
conda activate enviroment
```
&emsp;&emsp;上述指令就是用来进入这个虚拟环境的，例如`conda activate pytorch`就进入了名为`pytorch`的虚拟环境中了。当你进入这个环境的时候，命令行最左侧显示的就是当前所在的环境名称。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429163326341.png#pic_center)
&emsp;&emsp;有进入，那肯定有退出了，退出conda环境的指令是：
```python
# 退出conda环境
conda deactivate
```
&emsp;&emsp;在进入这个环境的时候，会默认装一些必要的包，那这个环境里装了什么包呢。
```python
# 查看环境中所装的包
conda list
```
&emsp;&emsp;然后安装包的话，指令是：
```python
# 安装包
conda install pkgs[==version]
```
&emsp;&emsp;你可以指定所安装软件包的版本号，如果不指定的话，就默认安装最新版本。例如需要安装python，则指令为`conda install python`，指定版本的方式就是`conda install python==2.7`，当然，其实在一开始创建环境的时候就可以指定python版本，具体指令为：
```python
# 新建包含Python的conda环境
conda create -n enviroment python==2.7
```
&emsp;&emsp;以上这些内容呢，就基本上是属于Anaconda的基本操作了，下面就要讲一些稍微复杂的了。
#### 镜像源添加
&emsp;&emsp;都知道，如果是外网的话，网速不保证，下载文件实在是太难了，所以呢，就有镜像源这么个东西，别人先帮你把东西下好，放在国内网站上，那肯定下起来就比从国外下方便多了，一般最常用的就是清华镜像源。`https://mirror.tuna.tsinghua.edu.cn/help/anaconda/`这个是清华镜像源的地址，里面介绍了使用方法，直接打开.condarc文件，然后修改里面的内容就可以了，将里面的内容修改为以下内容：
```python
# 清华镜像源.condarc文件修改
channels:
  - defaults
show_channel_urls: true
channel_alias: https://mirrors.tuna.tsinghua.edu.cn/anaconda
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/pro
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```
&emsp;&emsp;将文件内容进行更改后，输入`conda clean -i`，该指令的作用是清除索引缓存，当然实际上不输入也会更新。
#### Conda环境的克隆、复制、移植
&emsp;&emsp;假如说我现在已经建了一个名为`env_one`的环境，然后我想创建一个一模一样的，再把中间一个包升级到最新版本，如果我再重新这么建一遍新环境的话，就很麻烦，所以这时候就用到Conda的克隆功能，指令如下：
```python
# 依据就环境克隆一个新环境
conda create -n new_enviroment --clone old_enviroment
```
&emsp;&emsp;以上指令呢，就是将`old_enviroment`克隆到了`new_enviroment`中，你输入`conda env list`就可以看到`old_enviroment`和`new_enviroment`，然后你在`new_enviroment`里面瞎折腾都没关系，反正不会影响`old_enviroment`。
```python
# 删除环境
conda remove -n enviroment --all
```
&emsp;&emsp;上面这个命令呢，就是删除名为`enviroment`的环境了。

&emsp;&emsp;假如说你要把这个代码发到网上，你想要别人很快能够复原你的代码，这时候你可以上传你的环境，那怎么导出这个环境呢。
```python
# 导出环境
conda env export > environment.yml
```
&emsp;&emsp;执行完上面的指令后，就会在当前目录下生成一个名为enviroment.yml的文件，用记事本打开，可以看到里面的内容大概是这样的。
```python
# environment.yml内容
name: pytorch
channels:
  - defaults
dependencies:
  - atomicwrites=1.3.0=py37_1
  - attrs=19.3.0=py_0
  - blas=1.0=mkl
  - ca-certificates=2020.1.1=0
  - certifi=2020.4.5.1=py37_0
  - cffi=1.12.1=py37h2e261b9_0
  - cudatoolkit=9.0=h13b8566_0
  - cudnn=7.3.1=cuda9.0_0
  - importlib_metadata=0.23=py37_0
  - intel-openmp=2019.1=144
  - libedit=3.1.20181209=hc058e9b_0
  - libffi=3.3=he6710b0_1
  - libgcc-ng=8.2.0=hdf63c60_1
  - libgfortran-ng=7.3.0=hdf63c60_0
  - libstdcxx-ng=8.2.0=hdf63c60_1
  - mkl=2019.1=144
  - pip:
    - cycler==0.10.0
    - kiwisolver==1.0.1
    - matplotlib==3.0.3
    - opencv-python==3.4.2.17
prefix: /home/lijianquan/miniconda2/envs/pytorch
```
&emsp;&emsp;这里面就包含了你当前环境下所有的软件包， 可以看到这下面有`pip`，意思就是你在当前的环境下也是可以用`pip`的。

&emsp;&emsp;当用户下载了你的代码之后，又拿到了这个yml文件，就可以利用这个yml文件快速准确地复原环境了，指令如下：
```python
# 根据yml文件创建环境
conda env create -f environment.yml
```
&emsp;&emsp;那假如现在你的师弟，一个跟你用着同样的服务器，只不过是不同的用户名的人，也是用着同一个版本的Conda，问你要环境，你可以把enviroment.yml文件给他让他再重新安装，但这样慢啊，你们都用着同一个服务器，同一个版本的Conda，那有什么简单的方法呢，这里介绍两个。
##### 利用文件夹复制进行环境拷贝
&emsp;&emsp;首先第一个，不知道算不算歪门邪道，反正是自己捣鼓出来的，咱们看一下conda安装文件夹下的envs，这下面的文件夹就是咱们有的环境的名称，所以直接把整个文件夹复制到你师弟那儿不就行了嘛，这么一试，果然可行，指令如下：
```python
# 将环境文件夹拷贝到另一个用户下
scp -r /envs/[environment] username@[IP]:[address]/envs
```
&emsp;&emsp;这里用到`scp`指令，就是用在服务器之间传送文件的，只要知道对方账号的IP、账号以及密码就可以进行文件传送了，这里需要用到服务器`IP`，那如果是同一个服务器呢，就不需要IP了，之间用`localhost`就行。关于`scp`这个指令这里不过多解释，有兴趣的各位可以自行百度。
##### 利用conda pack进行环境拷贝
&emsp;&emsp;这第二种方法呢，就需要借助额外的打包工具，`conda pack`，这个打包工具必须安装在base下才能打包其他的环境。以下的两种安装方式任选其一就行。
```python
# 利用conda进行conda-pack包安装
conda install -c conda-forge conda-pack
# 利用pip进行conda-pack包安装
pip install conda-pack
```
&emsp;&emsp;安装完成之后呢，就可以进行环境打包了，指令如下：
```python
# 打包环境，生成environment.tar.gz
conda pack -n enviroment
# 打包环境，生成defined_name.tar.gz
conda pack -n my_env -o defined_name.tar.gz
# 打包环境，使生成的environment.tar.gz置于[path]下
conda pack -p [path]
```
&emsp;&emsp;然后打包的环境怎么安装呢，首先要在conda安装目录下的envs文件夹下新建一个文件夹，这个文件夹的名称就是环境的名称，即在`[path of conda]/envs/`下，然后用以下指令可以实现：
```python
# 定位到conda下的envs文件夹下
cd envs
# 新建用于环境的
mkdir enviroment
# 将打包的环境重新恢复
tar -xzf enviromen.tar.gz -C enviroment
```
&emsp;&emsp;打包有什么好处呢，就是方便移植，尤其是对于无法连接外网的服务器，这一点就非常重要了。

&emsp;&emsp;还有一点发现，直接通过scp过来的环境，就无法再使用下面的`conda pack`进行打包了，所以还是老老实实用`conda pack`进行环境打包比较好。

&emsp;&emsp;两种方法，前一种方法怎么说呢，比较歪门邪道，后面的就是正式的办法，还有就是`base`这个环境比较特殊，不能被打包，所以一般就不要去用这个环境，让它保持它纯洁的一面就行。
#### Conda安装本地文件
&emsp;&emsp;我们前面讲到，关于使用清华镜像源的问题，为什么要使用清华镜像源呢，就是因为服务器不稳定呗。所以也有一个问题，那就是清华镜像源也不稳定，所以就需要手动去下载，然后再手动安装本地文件。`https://anaconda.org/`这个是Anaconda包下载的地方，利用搜索框进行搜索，然后下载到服务器端。那有人会说，我是通过ssh，在Windows系统上连接到服务器端啊，这种怎么操作呢，提示一下用`scp`指令，过多的就不介绍了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200429214711312.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xvdmVfbGpx,size_1,color_FFFFFF,t_100)
&emsp;&emsp;下载完了之后，通过以下语句就可以进行软件包的安装了，下载下来的软件包，名字后缀是`.tar.bz2`。
```python
# 安装本地软件包
conda install --use-local package.tar.bz2
```
#### 总结
关于Anaconda的介绍暂时就这么多，为了便于大家查阅相关指令，我对主要指令进行了汇总

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;**→→→**[**传送门**](https://blog.csdn.net/love_ljq/article/details/105849776)
