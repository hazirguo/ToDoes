# C/C++ 中头文件相互包含引发的问题

今天下午遇到一个头文件相互包含而导致的编译问题，花了我不少时间去调试，最后晚上跟师兄讨论之后，终于有所顿悟！


## 问题重现

我写了一个函数 `bool func(ClassA* CA)` 需要加到项目中，我就把这个函数的声明放到 `head1.h` 中，函数参数类型 **ClassA** 定义在另一个头文件 `head2.h` 中，因此我需要在 `head1.h` 中包含 `head2.h`；而 `head2.h` 中之前又包含了 `head1.h`，这样就构成了一种头文件相互包含的场景。再加上一些其它的声明与定义，就构成了这样的一个文件结构：

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

