# tmux 笔记
## 说明
一个会话工具,主要解决ssh断开连接后任务继续执行.
也可实现工作保存的功能.

另外还有一个工具screen.看网上的比较,tmux更加新一点

主要参考这篇[博客](http://tangosource.com/blog/a-tmux-crash-course-tips-and-tweaks/)


# 编译安装
公司分的虚拟机tmux 只有1.6. 决定编译安装最新版,维护两套配置是很麻烦的.

1. 卸载
`apt-get remove tmux`
2. 装依赖
libevent2.x
`wget https://github.com/libevent/libevent/releases/download/release-2.1.8-stable/libevent-2.1.8-stable.tar.gz`
解压进入目录`./configure && make && make install`
3. 安装
`wget https://github.com/tmux/tmux/releases/download/2.5/tmux-2.5.tar.gz`
解压进入目录`./configure && make && make install`
`source ~/.basrc`或者重新登录

错误:运行tmux报`tmux: error while loading shared libraries: libevent-2.0.so.5: cannot open shared object file: No such file or directory`
解决:`ln -s /usr/local/lib/libevent-2.1.so.6 /usr/lib/libevent-2.1.so.6` 
感谢这篇[博客](http://blog.csdn.net/alvine008/article/details/48027687)不过似乎跟32还是64无关系

# 按键
ctrl-b 前缀键 按住松开再加其它键实现功能
`man tmux` 和 `ctrl-b ?`
## 会话
新建会话
`tmux new -s <name-of-my-session>` new-session缩写
`ctrl-b new -s <name-of-my-new-session>`
关闭退出会话
exit 命令或者 [Ctrl+d] 组合键，退出 tmux 会把会话结束掉
分离和连接会话
`ctrl-b d` detach
`tmux a -t <目标会话名>` attach-session

除非显示关闭,否则在计算机关闭前会话都不会消失
`tmux ls` `ctrl-b s`列出会话
`tmux attach` 连接上次退出的会话

`tmux kill-session -t` 干掉会话
## 窗口 window

`ctrl-b c`创建窗口
`ctrl-b [数字]`切换窗口 `n`后 `p`前
`ctrl-b &` 	关闭当前窗口
`ctrl-b ,` 	重命名当前窗口；这样便于识别
## 分屏 pane
`ctrl-b % `竖直分割
`ctrl-b "` 水平分割
`ctrl-b 方向键`切换
`ctrl-b x` 	关闭当前面板

## 命令行模式
`ctrl-b :`进入
`set mouse on[off]` 鼠标模式

## 复制 粘贴vi风格
`set-window-option -g mode-keys vi`
1) `Control+b [` 进入复制模式
2) `Space` 开始选择
4) `Enter` 结束选择
5) `Control+b ]` 粘贴复制的


`tmux show-buffer`
`tmux save-buffer foo.txt`

`Control + b #`
`tmux list-buffers`

`tmux show-buffer -b [buffer序号]`
`tmux save-buffer -b [buffer序号] foo.txt`

### 从本机剪贴板往tmux粘贴格式混乱问题
具体表现为有换行符的自动添加, 缩进混乱

**在tmux中直接command+V 本质上是模拟键盘字符挨个输入.所以在vim的normal模式下也可能呢键入字符**
使用vim的paste模式
`:set paste`
`:set nopaste`
当前快捷键
`<leader>pp`

### 实现从远程机器tmux复制内容到本机mac剪贴板
见[clipboard.md](clipboard.md)中的分析.
 


# 其它
`ctrl-b t` 显示时钟
`tmux -V` 版本号
`tmux`  启动
`tmux kill-server`  关闭

# TODO 
1. Tmux Resurrect  tmux-continuum保存会话(用于机器重启); 
2. Tmate 向其它人分享会话; 
3. Tmuxinator 定义会话模板 简化创建过程. 例如:每次会话都是一个编辑器和一个shell窗口
4. tmux-yank copy到系统剪贴板.

 