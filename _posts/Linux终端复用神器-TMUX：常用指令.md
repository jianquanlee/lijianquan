&emsp;&emsp;关于TMUX这个神器的详细讲解呢，另一篇文章中已经讲过了，这里为了方便大家查阅，专门对常用指令进行汇总。
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;**→→→**[**传送门**](https://blog.csdn.net/love_ljq/article/details/105791835)

#### Session操作
```python
# 创建session
tmux
```
```python
# 创建并指定session名字
tmux new -s session_name
```
&emsp;&emsp;如`tmux new -s proj`即为创建一个名为`proj`的session。
```python
# 临时退出session
Ctrl+B D
```
```python
# 列出session
tmux ls
```
```python
# 进入已存在的session
tmux a -t session_name
tmux at -t session_name
tmux attach -t session_name
```
&emsp;&emsp;这里需要强调的是，中间的字符串可以是`a`，`at`，`att`，`atta`，`attac`，`attach`，也就是任意`attach`的前面部分都可以。当然一般就用`a`或者`attach`就可以了。
```python
# 删除指定session
tmux kill-session -t session_name
```
&emsp;&emsp;如`tmux kill-session -t proj`这是删除名为`proj`的这个session。
```python
# 删除所有session
tmux kill-server
```
#### Window操作
```python
# 新建Window
Ctrl+B C
```
```python
# 重命名Window
Ctrl+B ,
```
```python
# 前一个Window
Ctrl+B P
```
```python
# 后一个Window
Ctrl+B N
```
&emsp;&emsp;这里的`P`和`N`分别代表的是`Previous`和`Next`，就是英文的前一个和后一个。
```python
# 删除Window
Ctrl+B &
```
#### Pane 操作
```python
# 竖向分屏
Ctrl+B %
```
```python
# 横向分屏
Ctrl+B "
```
```python
# 按照顺序在Pane之间切换
Ctrl+B O
```
```python
# 根据上下左右进行Pane切换
Ctrl+B ↑ ↓ ← →
```
```python
# 往左往上/往右往下进行Pane切换
Ctrl+B {
Ctrl+B }
```
```python
# 显示Pane编号
Ctrl+B Q
```
```python
# 删除当前Pane
Ctrl+B X
```
```python
# 调整Pane大小
Ctrl+B :resize-pane -U [距离]
Ctrl+B :resize-pane -D [距离]
Ctrl+B :resize-pane -L [距离]
Ctrl+B :resize-pane -R [距离]
```
```python
# 自动重新进行Pane排版
Ctrl+B Space
```
```python
# 将Pane升级为Window
Ctrl+B !
```
```python
# 将Pane合并到名为window_name的Window中
Ctrl+B :join-pane -t window_name
```
&emsp;&emsp;以上的指令基本上就够用了，如果还想了解更多的话，可以在[原网页](https://github.com/tmux/tmux/wiki)进行阅读，只不过英文的稍微难理解一点儿。
