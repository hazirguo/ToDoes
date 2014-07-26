# Zlib 安装与使用

Zlib 是一款开源的数据压缩/解压库，你可以在其 [官方网站](http://www.zlib.net/) 或者 [Github](https://github.com/madler/zlib) 上下载源码。


## Zlib 安装

Zlib 的安装非常简单，下载源码之后经过解压进入到解压之后的目录，你可以通过下面的步骤来安装：

``` bash
$ ./configure    #通常情况下，配置会建立静态和动态库，如果只想建立静态库，可以使用 ./configure --static
$ make test      #编译以及测试，如果测试通过，说明 zlib 库编译成功
$ make install   #安装到 /usr/local/lib/libz.* 和 /usr/local/include/zlib.h 
                 #如果想安装到 $HOME 而不是 /usr/local 下，使用
                 #make insatll prefix=$HOME
```


## Zlib 使用

Zlib 的使用也是非常简单的，所有的函数均包含在头文件 `zlib.h` 中，因此在你的程序加上此头文件。

编译我们的程序时，我们有两种方式将 zlib 编译进去：

* 静态链接：
* 动态链接：



## Zlib 举例



## Zlib API 说明


## Zlib 性能


## 参考文献

* Zlib 官网： http://www.zlib.net/
* Github Zlib 库： https://github.com/madler/zlib
