&emsp;&emsp;关于Anaconda这个神器的详细讲解呢，另一篇文章中已经讲过了，这里为了方便大家查阅，专门对常用指令进行汇总。
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;**→→→**[**传送门**](https://blog.csdn.net/love_ljq/article/details/105826720)
#### Anaconda的基本使用
```python
# 查询conda命令
conda -h
conda --help
```
```python
# 查看当前conda版本
conda --version
conda -V
```
```python
# 查看所有的conda环境
conda env list
conda info --envs
```
```python
# 新建conda环境
conda create -n enviroment
```
&emsp;&emsp;比如`conda create -n pytorch`就是建立了一个名为`pytorch`的环境
```python
# 新建包含Python的conda环境
conda create -n enviroment python==2.7
```
```python
# 进入conda环境
conda activate enviroment
```
```python
# 退出conda环境
conda deactivate
```
```python
# 查看环境中所装的包
conda list
```
```python
# 安装包
conda install pkgs[==version]
```
#### 镜像源添加
&emsp;&emsp;直接打开`.condarc`文件，然后修改里面的内容就可以了，将里面的内容修改为以下内容：
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
#### Conda环境的克隆、复制、移植
```python
# 依据就环境克隆一个新环境
conda create -n new_enviroment --clone old_enviroment
```
```python
# 删除环境
conda remove -n enviroment --all
```
```python
# 导出环境
conda env export > environment.yml
```
&emsp;&emsp;执行完上面的指令后，就会在当前目录下生成一个名为enviroment.yml的文件
```python
# 根据yml文件创建环境
conda env create -f environment.yml
```
##### 利用文件夹复制进行环境拷贝
```python
# 将环境文件夹拷贝到另一个用户下
scp -r /envs/[environment] username@[IP]:[address]/envs
```
&emsp;&emsp;以上指令可以直接进行环境拷贝，算是一种歪门邪道
##### 利用conda pack进行环境拷贝
&emsp;&emsp;conda pack必须安装在base下面
```python
# 利用conda进行conda-pack包安装
conda install -c conda-forge conda-pack
# 利用pip进行conda-pack包安装
pip install conda-pack
```
```python
# 打包环境，生成environment.tar.gz
conda pack -n enviroment
# 打包环境，生成defined_name.tar.gz
conda pack -n my_env -o defined_name.tar.gz
# 打包环境，使生成的environment.tar.gz置于[path]下
conda pack -p [path]
```
&emsp;&emsp;定位到conda下的envs文件夹下，并复制`enviromen.tar.gz`文件。
```python
# 定位到conda下的envs文件夹下
cd envs
# 新建用于环境的
mkdir enviroment
# 将打包的环境重新恢复
tar -xzf enviromen.tar.gz -C enviroment
```
#### Conda安装本地文件
&emsp;&emsp;在`https://anaconda.org/`网站搜索并进行下载。
```python
# 安装本地软件包
conda install --use-local package.tar.bz2
```
