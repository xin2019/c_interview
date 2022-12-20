# C语言面试宝典

### 什么是预编译？何时需要预编译？
预编译又称预处理，是整个编译过程最先做的工作，即程序执行前的一些预处理工作。主要处理#开头的指令。如拷贝#include包含的文件代码、替换#define定义的宏、条件编译#if等。.
何时需要预编译：

1、总是使用不经常改动的大型代码体。

2、程序由多个模块组成，所有模块都使用一组标准的包含文件和相同的编译选项。在这种情况下，可以将所有包含文件预编译为一个预编译头。

### 写一个“标准”宏，这个宏输入两个参数并返回较小的一个

#define MIN(x, y) ((x)<(y)?(x):(y)) //结尾没有;

### #与##的作用？
答：#是把宏参数转化为字符串的运算符，##是把两个宏参数连接的运算符。

例如：

#define STR(arg) #arg 则宏STR(hello)展开时为”hello”

#define NAME(y) name_y 则宏NAME(1)展开时仍为name_y

#define NAME(y) name_##y 则宏NAME(1)展开为name_1

#define DECLARE(name, type) typename##_##type##_type，

则宏DECLARE(val, int)展开为int val_int_type
### 如何避免头文件被重复包含？

答：

例如，为避免头文件my_head.h被重复包含，可在其中使用条件编译：
~~~
#ifndef _MY_HEAD_H
#define _MY_HEAD_H /*空宏*/
/*其他语句*/
#endif
~~~
