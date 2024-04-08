---
title: 分解helloworld
date: 2024-04-08 00:00:00 +0800
last_modified_at: 2024-04-08 00:00:00 +0800
categories: [cpp, syntax]
tags: [cpp]
author: author1
---

### 目录

#### 前言

C++基本语法一篇markdown每个知识点都记录太长，也难以查看，将不同部分分开，每个点详细更新一篇会好一点。
C++的语法和概念很多，一般视频教程也要2、30个小时。
更新顺序从博主认为的应该循序渐进的顺序来，和部分网站从变量等开始有所不同。

本想从命名空间开始详细更新，发觉分解helloworld.cpp会更有意思，一段简单的输出helloworld代码反而是很多
新手朋友容易忽略的地方，其实其中的命名空间、预处理`#`符等都值得更深入一点，这对理解一段cpp代码
如何被编译、执行更透彻。

```
// hello.cpp
#include <iostream>
// using namespace std;
int main() {
	std::cout << "Hello World" << std::endl;
	return 0;
}
```

### 正文

#### 头文件和预处理1

写代码和写文本是类似的，不可能将所有的东西都放在一个文件中，将不同功能等代码分解成各个模块存在文件中，
需要的时候引用即可，头文件也是这样，当然也不仅仅这么简单。

helloworld中用到了`cout`，实际上如若不引用`#include <iostream>`，`cout`在代码中不存在，因此要在开头将
`iostream`引用，`#`符是预处理符号，还记得编译代码的四个过程吗，预处理、编译、汇编、链接，在预处理过程
引用头文件的作用便出来了，`#include`会把`iostream`的代码复制一份到hello.cpp的开头位置。

可以想到，这其中可能存在问题，无法保证变量名、函数名等不会重复，因此要引出下方命名空间规则。

#### 命名空间

在一些输出helloworld例子中，开头有`using namespace std`，
这就是引入了一个叫`std`的命名空间，如果编写了这一行代码，在后续的调用`cout`中就不需要`std::cout`了，
即告诉了编译器接下来的`cout`在`std`这个空间中，实际上，查看iostream的源码会发现：
```
namespace std _GLIBCXX_VISIBILITY(default)
{
...
```

C++的源码将变量、函数等放到了一个叫`std`的命名空间中（其他文件也是如此）。

还记得gcc有一些参数可以仅做预处理、编译等四个过程吧，
假设在注释`// using namespace std;`的情况下只写`cout`显然会报错，试一试：
```
...
// using namespace std;
...
cout << "Hello World";
```

首先用`gcc -E test.cpp -o test.i`生成预处理文件
（注意`.i`结尾，**gcc --help中说的很清楚，会根据文件名后缀名决定编译器要做什么**），
这一步是不会报错的，因为预处理仅仅复制了一份代码，那接下来编译`gcc -S test.i -o test.s`，
这一步就会报错了：
```
...
error: ‘cout’ undeclared (first use in this function)
...
```

对编译器来说，`cout`是未声明的，因为它在不用`std::cout`显式的告诉编译器`cout`在哪的情况下，
它在预处理后的文件中找不到哪个地方声明了`cout`。

然后是一些语法，`using`不一定非要写在开头，写在需要调用的地方也可以，
也不一定非要`using`整个命名空间，`using std::cout`仅需使用的函数也是可以的。

编程方面，其实不推荐`using`命名空间，需要什么用`::`符号引出更易读。

#### 头文件和预处理2

