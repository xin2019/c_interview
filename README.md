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
### static关键字的作用？
Static的用途主要有两个，一是用于修饰存储类型使之成为静态存储类型，二是用于修饰链接属性使之成为内部链接属性。

1静态存储类型：

在函数内定义的静态局部变量，该变量存在内存的静态区，所以即使该函数运行结束，静态变量的值不会被销毁，函数下次运行时能仍用到这个值。

在函数外定义的静态变量——静态全局变量，该变量的作用域只能在定义该变量的文件中，不能被其他文件通过extern引用。

2 内部链接属性

静态函数只能在声明它的源文件中使用。
### const关键字的作用？
1声明常变量，使得指定的变量不能被修改。

const int a = 5;/*a的值一直为5，不能被改变*/

const int b; b = 10;/*b的值被赋值为10后，不能被改变*/

const int *ptr; /*ptr为指向整型常量的指针，ptr的值可以修改，但不能修改其所指向的值*/
~~~
 int a=4,b=5,c=6,d=7;
    const int* ptr;
    ptr=&a;
    cout<<*ptr<<endl;
    ptr=&b;
    cout<<*ptr<<endl; /* ptr的值可以修改 */
    *ptr=b; /* 但不能修改其所指向的值 *ptr 表示它指向的值*/
~~~

int *const ptr;/*ptr为指向整型的常量指针，ptr的值不能修改，但可以修改其所指向的值*/
~~~
int a=4,b=5,c=6,d=7;
    int *const ptr=&d;/*ptr为指向整型的常量指针，ptr的值不能修改，但可以修改其所指向的值*/
    *ptr=a;//合法
    ptr=&c;//非法
~~~

const int *const ptr;/*ptr为指向整型常量的常量指针，ptr及其指向的值都不能修改*/
~~~
 int a=4,b=5,c=6,d=7;
   const int *const ptr=&a;/*ptr为指向整型常量的常量指针，ptr及其指向的值都不能修改*/
   ptr=&d; //非法
   *ptr=c;//非法
~~~

2修饰函数形参，使得形参在函数内不能被修改，表示输入参数。

如int fun(const int a);或int fun(const char *str);

3修饰函数返回值，使得函数的返回值不能被修改。

const char *getstr(void);使用：const *str= getstr();

const int getint(void); 使用：const int a =getint();
