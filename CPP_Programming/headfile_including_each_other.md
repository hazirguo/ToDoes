# C/C++ 中头文件相互包含引发的问题

今天下午遇到一个头文件相互包含而导致的编译问题，花了我不少时间去调试没找到问题，最后晚上跟师兄讨论不少时间，突然有所顿悟！


## 问题重现

我把问题脱离于项目简单描述一下：我写了一个函数 `bool func(ClassA* CA)` 需要加到项目中，我就把这个函数的声明放到 `head1.h` 中，函数参数类型 **ClassA** 定义在另一个头文件 `head2.h` 中，因此我需要在 `head1.h` 中包含 `head2.h`；而 `head2.h` 中之前又包含了 `head1.h`，这样就构成了一种头文件相互包含的场景。再加上一些其它的声明与定义，就构成了这样的一个文件结构：

* **head1.h**

``` cpp
#ifndef __HEAD_1_H__
#define __HEAD_1_H__

#include "head2.h"

#define VAR_MACRO  1          //define a macro, which used in head2.h

bool func(ClassA* CA);        //ClassA is defined in head2.h

#endif 
```

* **head2.h**

``` cpp
#ifndef __HEAD_2_H__
#define __HEAD_2_H__

#include "head1.h"

class ClassA{
  int mVar;
  void setMem(){ mVar = VAR_MACRO };    //macro VAR_MACRO is defined in head1.h
  
  ...     //other members and functions
};

#endif 
```

那么，现在另有两个源文件 

* **source1.cpp** 

``` cpp
#include "head1.h"

//... some source code

```

* **source2.cpp**

``` cpp
#include "head2.h"

//... some source code

```

整个项目会分别编译这两个源文件，编译完之后会报错，大致意思是 **ClassA 和 VAR_MACRO 没有定义**，那么问题就比较奇怪了，每个头文件都分别引用了另一个头文件，为什么会出现未定义呢？


## 问题分析

我们都知道 C/C++ 中头文件开始习惯使用 `#ifndef ... #define ... #endif` 这样的一组预处理标识符来防止重复包含，例如上面的问题中我也使用了，如果不使用的话，两个头文件相互包含，就出现递归包含。这个很好理解就不多叙。

回到问题本身，我在微博上贴出了这个[问题](http://weibo.com/1678478585/Bir80BF69)，有人说在 `head1.h` 的函数前加上 `ClassA A;` 的前置声明，至于为什么要加，估计很多人都没理解...

对于 `source1.cpp`，它包含了头文件 `head1.h`，那么在编译之前，在 `source1.cpp` 中展开 `head1.h`，而 `head1.h` 又包含了 `head2.h`， 那么也展开它，这时 `source1.cpp` 就变成类似下面这样：

``` cpp
class ClassA{
  int mVar;
  void setMem(){ mVar = VAR_MACRO };    //macro VAR_MACRO is defined in head1.h
  
  ...     //other members and functions
};

#define VAR_MACRO  1          //define a macro, which used in head2.h

bool func(ClassA* CA);        //ClassA is defined in head2.h

//... source1.cpp source code
```

看到没，这地方 **func** 函数之前有 **ClassA** 类型的定义，根本没必要像有些人说的那样加上 `ClassA CA;` 这样的前置声明。

我们再展开 `source2.cpp` 看看：

``` cpp
#define VAR_MACRO  1          //define a macro, which used in head2.h

bool func(ClassA* CA);        //ClassA is defined in head2.h

class ClassA{
  int mVar;
  void setMem(){ mVar = VAR_MACRO };    //macro VAR_MACRO is defined in head1.h
  
  ...     //other members and functions
};

//... source2.cpp source code
```

这时问题就很清楚了，**func** 函数声明之前并没有发现 **ClassA** 类型定义，该定义在函数声明的后面，这时候如果能在 `head1.h` 的函数声明之前加上 `ClassA CA;` 的前置声明，就不会在编译的时候报找不到 ClassA 的定义的错误了。

再回到 `source1.cpp` 展开的源码看看，是不是一下子明白了为什么报找不到 VAR_MACRO 的定义的错误了？修改方法也简单，把宏定义拉到 `#include "head2.h"` 语句之前就 OK 了。


## 问题反思

现在回头想想这个问题，其实是个非常简单的头文件包含的问题，如果了解一些编译器的预编译过程，错误原理也很简单。但为什么我卡在这个问题很长时间，原因有以下几点：

* 问题出现在一个项目的编译过程中，未能准确地定位问题的原因
* 出现问题，没有静下来心来理清问题，C/C++ 编译基础知识点虽然知道，但未能第一时间运用到该问题的分析上
* 师兄说加上前置类的声明，确实解决了类类型找不到的错误，虽然有问题解决方法，但没有真正理解这种做法的道理（估计师兄也没理解，偷笑ing...），以至于宏定义找不到的错误还是不知如何去解决（其实本质是一样的）

总的来说，遇到问题不要慌，保持大脑清醒，把加一行或减一行代码期望就能碰运气编译通过的时间拿来分析问题更有效，解决问题之后一定确定自己是否知其然亦知其所以然！


## 参考资料

Google找了一圈，没找到有价值的资料....



