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
### volatile关键字的作用？
volatile指定的关键字可能被系统、硬件、进程/线程改变，强制编译器每次从内存中取得该变量的值，而不是从被优化后的寄存器中读取。例子:硬件时钟;多线程中被多个任务共享的变量等。
### extern关键字的作用？
1用于修饰变量或函数，表明该变量或函数都是在别的文件中定义的，提示编译器在其他文件中寻找定义。
~~~
extern int a;
extern int *p;
extern int array[];
extern void fun(void);
~~~
其中，在函数的声明带有关键字extern，仅仅是暗示这个函数可能在别的源文件中定义，没有其他作用。如
头文件A：A_MODULE.h中包含

extern int func(int a, int b);

源文件A: A_MODULE.c中
~~~
#include “A_MODULE.h”
int func(int a, int b)
{
 returna+b;
}
~~~
此时，展开头文件A_MODULE.h后，为

extern int func(int a, int b);/*虽然暗示可能在别的源文件中定义，但又在本文件中定义，所以extern并没有起到什么作用，但也不会产生错误*/
~~~
int func(int a, int b)
{
 returna+b;
}
而源文件B：B_MODULE.c中，
#include “A_MODULE.h”
int ret = func(10,5);/
展开头文件A_MODULE.h后，为
extern int func(int a, int b);/*暗示在别的源文件中定义，所以在下面使用func(5,10)时，在链接的时候到别的目标文件中寻找定义*/
int ret = func(10,5);
~~~
2 用于extern “c

extern “c”的作用就是为了能够正确实现C++代码调用其他C语言代码。加上extern "C"后，会指示编译器这部分代码按C语言的编译方式进行编译，而不是C++的。

C++作为一种与C兼容的语言，保留了一部分面向过程语言的特点，如可以定义不属于任何类的全局变量和函数，但C++毕竟是一种面向对象的语言，为了支持函数的重载，对函数的编译方式与C的不同。例如，在C++中，对函数void fun(int,int)编译后的名称可能是_fun_int_int，而C中没有重载机制，一般直接利用函数名来指定编译后函数的名称，如上面的函数编译后的名称可能是_fun。

这样问题就来了，如果在C++中调用的函数如上例中的fun(1,2)是用C语言在源文件a_module.c中实现和编译的，那么函数fun在目标文件a_module.obj中的函数名为_fun，而C++在源文件b_module.cpp通过调用其对外提供的头文件a_module.h引用后，调用fun，则直接以C++的编译方式来编译，使得fun编译后在目标文件b_module.obj的名称为_fun_int_int，这样在链接的时候，因为_fun_int_int的函数在目标文件a_module.obj中不存在，导致了链接错误。

解决方法是让b_module.cpp知道函数fun是用C语言实现和编译了，在调用的时候，采用与C语言一样的方式来编译。该方法可以通过extern “C”来实现（具体用法见下面）。一般，在用C语言实现函数的时候，要考虑到这个函数可能会被C++程序调用，所以在设计头文件时，应该这样声明头文件：
~~~
/*头文件a_module.h*/
/*头文件被CPP文件include时，CPP文件中都含有该自定义的宏__cplusplus*/
/*这样通过extern “C”告诉C++编译器，extern “C”{}里包含的函数都用C的方式来编译*/
#ifdef __cplusplus 
extern “C”
{
#endif
extern void fun(int a, int b);
#ifdef __cplusplus
}
#endif
~~~
extern "C"的使用方式

1. 可以是单一语句

extern "C" doublesqrt(double);

2. 可以是复合语句, 相当于复合语句中的声明都加了extern "C"
~~~
extern "C"
 {
 double sqrt(double);
 int min(int, int);
 }
~~~
3.可以包含头文件，相当于头文件中的声明都加了extern"C"
~~~
 extern "C"
  {
    #include <cmath>
  }
~~~
4. 不可以将extern"C" 添加在函数内部
5. 如果函数有多个声明，可以都加extern"C", 也可以只出现在第一次声明中，后面的声明会接受第一个链接指示符的规则。

6. 除extern"C", 还有extern "FORTRAN" 等。
### sizeof关键字的作用？
sizeof是在编译阶段处理，且不能被编译为机器码。sizeof的结果等于对象或类型所占的内存字节数。sizeof的返回值类型为size_t
变量：int a; sizeof(a)为4；

指针：int *p; sizeof(p)为4；

数组：int b[10]; sizeof(b)为数组的大小，4*10；

int c[0]; sizeof(c)等于0

结构体：struct (int a; char ch;)s1; sizeof(s1)为8 与结构体字节对齐有关。即以结构体成员中占内存最多的数据类型所占的字节数为标准，所有的成员在分配内存时都要与这个长度对齐

注意：不能对结构体中的位域成员使用sizeof

sizeof(void)等于1

sizeof(void *)等于4
### 结构体的赋值？
C语言中对结构体变量的赋值或者在初始化或者在定义后按字段赋值。

方式1：初始化
~~~
struct tag
{
 chara;
 int b;
}x = {‘A’, 1};/*初始化*/
或
struct tag
{
char a;
int b;
};
struct tag x = {‘A’,1};/*在定义变量时初始化*/
~~~
GNU C中可使用另外一种方式：
~~~
struct tag
{
char a;
int b;
}x =
{
.a = ‘A’,
.b =1;
};
或
struct tag
{
char a;
int b;
};
struct tag x =
{
 .a= ‘A’,
 .b=1,
};
~~~
方式2：定义变量后按字段赋值
~~~
struct tag
{
char a;
int b;
};
struct tag x;/*定义变量*/
x.a = ‘A’;/*按字段赋值*/
x.b = 1; /*按字段赋值*/
~~~
而当你使用初始化的方式来赋值时，如x = {‘A’,1};则出错。

方式3：结构变量间的赋值
~~~
struct tag
{
 chara;
 int b;
};
struct tag x,y;
x.a=’A’;
x.b=1;
y = x;/*结构变量间直接赋值*/
~~~
### 结构体变量如何比较？
虽然结构体变量之间可以通过=直接赋值，但不同通过比较符如==来比较，因为比较符只作用于基本数据类型。这个时候，只能通过int memcmp(const void *s1, const void *s2, size_t n);来进行内存上的比较。
### 结构体位域
位域是一个或多个位的字段，不同长度的字段（如声明为unsigned int类型）存储于一个或多个其所声明类型的变量中（如整型变量中）。

位域的类型：可以是char、short、int，多数使用int，使用时最好带上signed或unsigned

位域的特点：字段可以不命名，如unsignedint :1;可用来填充；unsigned int :0; 0宽度用来强制在下一个整型（因此处是unsigned int类型）边界上对齐。

位域的定义：
~~~
struct st1
{
unsigned chara:7;/*字段a占用了一个字节的7个bit*/
unsigned charb:2;/*字段b占用了2个bit*/
unsigned charc:7;/*字段c占用了7个bit*/
}s1;
~~~
sizeof(s1)等于3。因为一个位域字段必须存储在其位域类型的一个单元所占空间中,不能横跨两个该位域类型的单元。也就是说，当某个位域字段正处于两个该位域类型的单元中间时，只使用第二个单元，第一个单元剩余的bit位置补（pad）0。

于是可知Sizeof(s2)等于3*sizeof(int)即12
~~~
struct st2
{
unsigned inta:31;
unsigned intb:2;/*前一个整型变量只剩下1个bit，容不下2个bit，所以只能存放在下一个整型变量*/
unsigned int c:31;
}s2;
~~~
位域的好处：

1.有些信息在存储时，并不需要占用一个完整的字节， 而只需占几个或一个二进制位。例如在存放一个开关量时，只有0和1 两种状态，用一位二进位即可。这样节省存储空间，而且处理简便。这样就可以把几个不同的对象用一个字节的二进制位域来表示。
2.可以很方便的利用位域把一个变量给按位分解。比如只需要4个大小在0到3的随即数，就可以只rand()一次，然后每个位域取2个二进制位即可，省时省空间。

位域的缺点：

不同系统对位域的处理可能有不同的结果，如位段成员在内存中是从左向右分配的还是从右向左分配的，所以位域的使用不利于程序的可移植性。
### 结构体成员数组大小为0
结构体数组成员的大小为0是GNU C的一个特性。好处是可以在结构体中分配不定长的大小。如
~~~
typedef struct st
{
 inta;
int b;
char c[0];
}st_t;
sizeof(st_t)等于8，即char c[0]的大小为0.
#define SIZE 100
st_t *s = (st_t *)malloc(sizeof(st_t) + SIZE);
~~~

