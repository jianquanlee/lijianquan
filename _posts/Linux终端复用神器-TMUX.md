&emsp;&emsp;正所谓，工欲善其事必先利其器，一个好的工具能够极大地提高工作效率，今天就来介绍一下**Linux复用神器，TMUX**。

&emsp;&emsp;这是怎样一个工具呢，需要在什么条件下用到呢，首先来解决这个疑惑。当你在用服务器的时候，如果在安装package，下载速度又很慢，就感觉啥事也做不了，只能干等着；当你在跑网络或者下载东西时，实验室的网断了，是不是浑身一抖，程序就这么结束了；当你想要通过vim打开两个文件进行查看时，发现打开一个必须关闭另一个时，TMUX这个神器就开始展现他应有的魅力了~

&emsp;&emsp;这是一个终端复用神器，什么意思呢，可以这么来理解，你通过SSH工具连接Linux服务器，就一个界面，就像在Windows系统下，聊微信或者上网，你只能选择一样，你聊着微信，突然想上网了，把微信关了，打开浏览器，也就是单进程的，这样就很耽误事是吧。TMUX就可以让你玩着游戏，一会儿呢切换到浏览器看看，再一会儿呢切回到微信。

&emsp;&emsp;首先是TMUX的安装，`https://github.com/tmux/tmux/wiki`这个是[TMUX的网页](https://github.com/tmux/tmux/wiki)，上面有讲到如何进行TMUX安装，这个安装呢就不再过多介绍了，当然他跟你具体的Linux版本也有关系。当然关于这个tmux的所有内容这个它的页面上都有，主要是吧，这用英文介绍的，读起来也不是特别方便，网上有很多关于tmux这个软件的使用方法，只不过呢，他就是拿过来让你记住，什么什么指令是什么含义，但是吧，这些指令那么复杂，如果不搞清楚或者产生什么记忆点的话，很容易就忘记了，这跟我们学英语也是一样的道理。比如你认识try这个单词，那retry什么意思呢，老师教过你，re-这个前缀的含义就是再次的意思，所以retry就是再次尝试的意思。再比如电脑上复制的快捷键是`Ctrl+C`，如果告诉你C means Copy，是不是就一下子记住了呢。当然这些内容TMUX的页面上都有，这里呢，也只是做一个简单的总结罗列。
#### Session操作
```python
# 首先是创建session
tmux
```
&emsp;&emsp;这里来介绍一下session，session翻译成中文呢，有一节的意思，你可以理解成是开一个浏览器，你的电脑可以同时打开多个浏览器。
```python
# 创建并指定session名字
tmux new -s session_name
```
&emsp;&emsp;上面那个呢，是创建一个session，但是你没有给它起名字，系统就给他起了一个名字，叫做`0`，就像你新建一个文件夹，不取名字，他的默认名称就是`新建文件夹`，这里的指令呢，就是创建一个带名字的session，它的名字就是`session_name`。可以看到，这个指令里面有个`-s`，这个`s`的意思`session`。好了，浏览器打开了，那我们就可以开始学习一些新的指令了。
```python
# 临时退出session
Ctrl+B D
```
&emsp;&emsp;这个指令呢，就是暂时退出session，怎么操作呢，同时按下`Ctrl`和`B`，然后松开，接着按下`D`。这里的`D`就是`detach`的意思，即为分离。
```python
# 列出session
tmux ls
```
&emsp;&emsp;这个指令呢，就是你在主界面，也就是没有进入到tmux的session的情况下，列出所有的session的，这里的`ls`呢，很明确就是`list`的意思。好了，我们想要进入第一个session，这个指令呢就是
```python
# 进入已存在的session
tmux a -t session_name
tmux at -t session_name
tmux attach -t session_name
```
&emsp;&emsp;这里呢，我写了三个，其实这三个指令的含义都是一样的，就是进入已存在的session，测试的时候呢，发现中间的字符串`a`，`at`，`attach`都行，后面又试了一下`att`，`atta`这些都是可以的，也就是只要是`attach`这个字符串的前几位都可以。
```python
# 删除指定session
tmux kill-session -t session_name
```
&emsp;&emsp;这个指令的意思就是删除名为`session_name`的这个session，那假如说我们要删除全部的session用什么指令呢，就用下面这个。
```python
# 删除所有session
tmux kill-server
```
#### Window操作
&emsp;&emsp;讲到这里呢，我们基本上都是在界面外面介绍，下面呢，就进入到session里面，讲讲里面的具体操作。看到默认的窗口名字不好看，也没啥特殊含义，咱先来改个名
```python
# 重命名Window
Ctrl+B ,
```
&emsp;&emsp;是的，这个`,`是逗号，跟前面一样，同时按下`Ctrl`和`B`，然后松开，接着按下`,`。下图就是进行Window重命名的样子，背景的颜色由原先的绿色变成了黄色，

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427194521900.png#pic_center)
&emsp;&emsp;好了，这个Window用来测试yolo，那如何在当前的Window后面再新建一个呢，用的呢就是下面这个代码。
```python
# 新建Window
Ctrl+B C
```
&emsp;&emsp;这个新建的Window的名称默认是bash，我们可以给他进行改名，现在呢，我们已经建了三个Window了。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427195059186.png#pic_center)
&emsp;&emsp;那要怎么进行Window切换呢，指令如下：
```python
# 前一个Window
Ctrl+B P
```
```python
# 后一个Window
Ctrl+B N
```
&emsp;&emsp;这里的`P`和`N`分别代表的是`Previous`和`Next`，就是英文的前一个和后一个，知道了单词的含义，记起来就方便了。还有一点要注意的是，这个一个循环，在最后一个Window那里切换到Next的时候，其实就是切换到第一个。
&emsp;&emsp;如果多建了一个Window，想要删掉，那么指令就是
```python
# 删除Window
Ctrl+B &
```
&emsp;&emsp;需要注意的是，一般你的`&`是在数字`7`上面的，也就是说，你得同时按下`Shift`按键。
#### Pane 操作
&emsp;&emsp;你可以多Window之间切换，但是你要抄代码还是不方便啊，这时候呢，就要讲到一个东西，Pane，翻译成中文，叫窗格。你把一个显示屏分成左右两部分，那抄代码岂不是飞起，下面就讲讲这些指令。
```python
# 竖向分屏
Ctrl+B %
```
```python
# 横向分屏
Ctrl+B "
```
&emsp;&emsp;这里的`%`就是数字`5`上面的符号，也就是要按下`Shift`和`5`。这里的`"`就是标点`'`上面的符号，也就是要按下`Shift`和`'`。下图就是进行竖向和横向分屏之后的结果。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427201501736.png)
&emsp;&emsp;已经有三个Pane呢，那三个Pane之间怎么切换呢。
```python
# 按照顺序在Pane之间切换
Ctrl+B O
```
&emsp;&emsp;一个个轮转过来，光标在哪里，就说明当前是哪个Pane处于Activate状态。当然你也可以用小键盘的上下左右。
```python
# 根据上下左右进行Pane切换
Ctrl+B ↑ ↓ ← →
```
&emsp;&emsp;还有个切换的指令，这个切换就有一定的方向性，不过要注意的是，Pane是循环的，你到了最右边最下面的，再往右往下，就是最左上角那个了。
```python
# 往左往上/往右往下进行Pane切换
Ctrl+B {
Ctrl+B }
```
&emsp;&emsp;上面说到，按照顺序在Pane之间切换，那怎么知道Pane的顺序呢，就按照下面这个指令。下面这幅图呢，显示的就是Pane的编号顺序。
```python
# 显示Pane编号
Ctrl+B Q
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427202951900.png)
```python
# 删除当前Pane
Ctrl+B X
```
&emsp;&emsp;要注意的是，删除Pane之后，Pane的编号可能发生了变化。那如果我觉得这个Pane太小了，该怎么调整呢，
```python
# 调整Pane大小
Ctrl+B :resize-pane -U [距离]
Ctrl+B :resize-pane -D [距离]
Ctrl+B :resize-pane -L [距离]
Ctrl+B :resize-pane -R [距离]
```
&emsp;&emsp;这里分别是`上(Up，-U)`，`下(Down，-D)`，`左(Left，-L)`，`右(Right，-R)`进行Pane尺寸调整的指令，这里的`[距离]`是可选项，或者认为默认值是1也行。这里要注意的是，这个操作跟修改Window名称很像，就是在下面的command栏里输入文字。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200427204722105.png#pic_center)

&emsp;&emsp;还有可以让系统自动进行Pane排版，你随便建5个Pane，然后按照下面的指令，系统会帮你排版，你一直按，排版格式会不断变化，你直接挑选自己喜欢的那个就行。
```python
# 自动重新进行Pane排版
Ctrl+B Space
```
&emsp;&emsp;这里的`Space`就是空格键。
```python
# 将Pane升级为Window
Ctrl+B !
```
&emsp;&emsp;上述指令呢，就可以将Pane升级为Window，直接Window后面就多一个新的Window，内容就是这个Pane。那如果我不要新建一个Window，而是将这个Pane合并到已经存在的Window上呢，办法也是有的。
```python
# 将Pane合并到名为window_name的Window中
Ctrl+B :join-pane -t window_name
```
&emsp;&emsp;这个指令的操作和调整Pane大小的指令类似，也是在底下的Command栏中进行的。
&emsp;&emsp;以上的指令基本上就够用了，如果还想了解更多的话，可以在[原网页](https://github.com/tmux/tmux/wiki)进行阅读，只不过英文的稍微难理解一点儿。为了便于大家查阅，我写了另一篇博客，将这些指令进行了一个汇总。

&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;**→→→**[**传送门**](https://blog.csdn.net/love_ljq/article/details/105796556)
