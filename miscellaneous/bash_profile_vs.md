# .bash_profile vs .bashrc

当我使用 Linux 的时候，需要为我的 shell 设置 `PATH` 和其它环境变量，我总是不知道应该修改哪个配置文件，应该编辑我的 home 目录下的 `.bash_profile` 还是 `.bashrc` 呢？

你可以将配置放在这两个中的任何一个，如果它不存在，你还可以创建出来，但是为什么有两个不同的文件呢？它们的区别又是什么？

根据 [bash man page]()，`.bash_profile` 是在 login_shells 下执行，而 `.bashrc` 实在交互式 non-login shells 下执行。


## 什么是 login 和 non-login shells 呢？

当你通过终端（console）使用你的用户名和密码 **`login`** 时，无聊是直接坐在主机前还是远程通过 ssh 来连接，在初始化命令提示符之前，会执行 `.babsh_profile` 来配置你的 shell。

但是，如果你已经登录到你的机器，在 Gnome 或者 KDE 中打开一个新的终端窗口（terminal window），那么在窗口提示符出现之前会执行 `.bashrc` 。同时，当你在一个终端中键入 `/bin/bash` 启动一个新的 bash 实例时，它也会执行。


## 为什么有这两个不同的文件？

假设，你想在你登录你的机器时打印出长长的机器诊断信息，如平均负载、内存使用、当前用户等等，而你 **仅仅** 想在你登录的时候看到，这时候你只需要将这个放到你的 `.bash_profile` 文件中。如果你将它放到 `.bashrc` 中，你将在每次打开一个终端窗口时都会看到这样的信息。

## 推荐

大多时候你并不想为 login 和 non-login shells 维护两个单独的配置文件，当你设置 `PATH` 时，你想将此应用到两个中，你可以在 `.bash_profile` 文件中引入 `.bashrc`，然后将 `PATH` 和一些公用的配置放入到 `.bashrc` 中。

为了做到这样，需要在 `.bash_profile` 中添加下面的代码：

``` bash
if [ -f ~/.bashrc ]; then
   source ~/.bashrc
fi
```

现在当你从 console 中登录你的机器时，`.bashrc` 也会被调用了。


-----

编译自： http://www.joshstaiger.org/archives/2005/07/bash_profile_vs.html
