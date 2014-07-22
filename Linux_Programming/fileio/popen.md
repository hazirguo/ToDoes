# popen 与 pclose


## 函数原型

``` c
#include <stdio.h>

FILE * popen (const char * command , const char * type);
int pclose (FILE * stream);
```

## 函数说明

**`popen`** 函数创建一个子进程，子进程执行 shell 命令，由参数 **command** 指定，子进程与调用 popen 函数的父进程通过管道连接，由于管道是单向传输数据的，方向由参数 **type** 决定。该函数成功时返回可读/写管道的文件流指针，失败返回 NULL，并且错误代码写入到 errno 中。popen 函数打开的管道文件流只能由 **`pclose`** 函数关闭，该函数等待命令执行结束，关闭文件流，失败时返回 -1，错误代码写入 errno 中。

* 当 type 设置为 “r” （读）时，从管道文件流中读数据就是读命令的标准输出，而命令的标准输入和调用 popen 函数的进程标准输入相同。
* 当 type 设置为 “w” （写）时，往管道文件流中写数据就是写命令的标准输入，而命令的标准输出和调用 popen 函数的进程标准输出相同。


## 举例

### popen 读入的例子

先创建一个 test 文件，往里面写入一些数据，用下面的程序执行之后，会输出文件里的内容：

``` c
#include <stdio.h>

#define MAX_LEN  100
int main()
{
    FILE *fp;
    char buf[MAX_LEN];
    if ((fp = popen("cat test", "r")) == NULL)
    {
        perror("fail to popen");
        exit(1);
    }

    while(fgets(buf, MAX_LEN, fp) != NULL)
    {
        printf("%s", buf);
    }

    if (pclose(fp) == -1)
    {
        perror("fail to pclose");
        exit(1);
    }

    return 0;
}
```

### popen 写入的例子

执行下面的程序，会往 test1 文件中写入 “helloworld” 字符串：

``` c
#include <stdio.h>

#define MAX_LEN 100
int main()
{
    FILE* fp = NULL;
    char buf[MAX_LEN];
    if ((fp = popen("cat > test1", "w")) == NULL)
    {
        perror("fail to popen");
        exit(1);
    }

    fwrite("helloworld", 1, sizeof("helloworld"), fp);

    if (pclose(fp) == -1)
    {
        perror("fail to pclose");
        exit(1);
    }

    return 0;
}
```


## 参考文献

* [man 手册](http://man7.org/linux/man-pages/man3/popen.3.html)
* [网络博文，唯一一篇看到举了写例子的文章](http://linux.chinaitlab.com/c/806015.html)
