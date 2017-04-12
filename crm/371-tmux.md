# Tmux

#### 作者：江伟

#### 邮箱：jiangwei@haomo-studio.com

##简介

tmux是指通过一个终端登录远程主机并运行后，在其中可以开启多个控制台而无需再“浪费”多余的终端来连接这台远程主机的软件。

##特点

1. 一个终端多个会话（session）
2. 允许随时随地断开或重新接入会话

##下载
####在 Mac OS 中安装
- 安装 Homebrew
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
- 安装 Tmux
```
$ brew install tmux
```
####在 Ubuntu 中安装
在终端输入如下命令
```
sudo apt-get install tmux
```



##快捷键

#### 快捷键前缀
为了使自身的快捷键和其他软件的快捷键互不干扰，Tmux 提供了一个快捷键前缀。当想要使用快捷键时，需要先按下快捷键前缀，然后再按下快捷键。Tmux 所使用的快捷键前缀默认是组合键 Ctrl-b（同时按下 Ctrl 键和 b 键）。例如，假如你想通过快捷键列出当前 Tmux 中的会话（对应的快捷键是 s），那么你只需要做以下几步：

1. 按下组合键 Ctrl-b (Tmux 快捷键前缀)
2. 放开组合键 Ctrl-b
3. 按下 s 键

#### 运行TMUX

好了，下面让我们从运行 tmux 开始，在终端运行如下的命令，就开启啦一个tmux会话。
```
$ tmux
```
#### 显示所有会话
由于 tmux 的理念是可以开启多个会话，并且可以自由地断开会话后重新接入，为此我们需要首先能看到可用的会话。有两种方法可以实现这个目的：

```
# 使用快捷键（默认为 Ctrl-b）

$ Ctrl-b s
```
```
# 使用 tmux 的子命令

$ tmux ls
```
> 注意：第二种方法可能会存在`sessions should be nested with care, unset $TMUX to force`的问题，需要在会话中说输入`unset TMXU`,才能使用`tmux`命令。

上面两种方法的效果相同，都可以得到类似下面的结果：
```
0: 1 windows (created Thu Nov 28 06:12:52 2013) [80x24] (attached)
```
####新建会话
下面我们就来新建一个会话。可以使用 new 命令新建会话，并且该命令允许以参数的形式传递一个会话名。我的建议是在新建时要提供一个会话名以便于日后管理
```
$ tmux new -s session-name
```
####接入一个之前的会话
可以通过参数指定一个想接入的会话：
```
$ tmux a -t session-name
```
####从会话中断开
可以使用 detach 命令断开已有的会话
```
$ tmux detah
```
也可以使用快捷键断开会话：

```
$ Ctrl-b d
```
####关闭会话
```
$ tmux kill-session -t seesion-name
```
####重新接入断开的会话
```
$ tmux attach
```
####创建window窗口
```
Ctr+b  c
```
####关闭window
```
Ctr+b  &
```
####创建pane
```
左右分
Ctr+b %
上下分
Ctr+b "  
```
####关闭pane
```
关闭当前pane
Ctr+b x
关闭所有pane
Ctr+b !
```