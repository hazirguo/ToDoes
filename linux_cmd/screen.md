# screen

当我们在 Linux 下使用终端时，是不是经常开多个窗口并发地工作，例如在编译一个大的项目需要的时间很长，我们通常需要再开一个终端干点其它的事情。如果我们使用类似于 putty 这样的软件，通过 ssh 协议远程到服务器上，如果开多个 putty 窗口，显然这不是一个很好的做法，那怎样做到在一个 putty 窗口下实现多终端的切换呢？

我们可以使用 **screen** ！


## 基本操作

* 创建一个 screen 窗口
* 在 screen 窗口之间来回切换
* screen 窗口与终端窗口之间来回切换


## 命令

* screen ： 创建一个新的 screen 窗口
* screen -ls ： 显示所有的 screen 窗口
* screen -r *pid* ：后接 screen 进程号切换到该 screen 窗口


## screen 窗口下的快捷键

|快捷键 | 说明 |
|-------|------|
|Ctrl+A  **C** reate | 创建一个新的 screen 窗口会话 |
|Ctrl+A  **D** etached |  暂时断开 screen 窗口会话 |
|Ctrl+A  **W** atch | 显示所有 screen 窗口 |
|Ctrl+A  **0-9** | 切换到 0-9 号 screen 窗口 |
|Ctrl+A  **Ctrl+A** | 发送 Ctrl+A 信号到 screen 窗口 |
|Ctrl+A  **K** ill | 杀死当前 screen 窗口 |

