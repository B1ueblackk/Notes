# C++笔记

## 概述

### 语言

语法，语义，语用

例 thank sb for sth sb 和 sth 不是句子中实际存在的 但可以进行替换

Backus–Naur Form EBNF/语法图

```
<IDENTIFIE> ::= <A> |
	       <IDENTIFIER> <AD> 
<AD> ::= <A>|<D>|
                <AD><A>|
                <AD><D>
 
<A>(非终极符号VN) ::= _|a|b| …… X|Y|Z(VT)
<D> ::= 0| ……| 8 | 9 

letter = [a-zA-Z]
digit = [0-9]
identifier = letter (letter|digit)*
```

**语言的定义**：特定字母表上，按照一定的语法规则组成的符号串集合

### Programming

发展史上两种观点：The Science of programming & The Art of Computer Programming

```java
//Science角度
//求商和余数
r:=x;   q:=0;
while  r>y 
 begin
    r = r-y;
    q:=q+1;
end;
//这个版本如果y小于0则会一直跑下去，没有从Science角度完善好
{y>0}(前置条件)
r:=x;   q:=0;
while  r>y 
 begin
    r = r-y;
    q:=q+1;
end;
{x=y*q+r}(后置条件)
//不满足整除情况
{ y>0}
r:=x;   q:=0;
while  r ≥ y 
 begin
    r = r-y;
    q:=q+1;
end;
{ x=y*q+r and r<y}
//继续推演
{ x ≥ 0 and y>0}
r:=x;   q:=0;
while  r ≥ y 
 begin
    r = r-y;
    q:=q+1;
end;
{ x=y*q+r and r<y}
```

## 结构化程序设计 process programming

程序 -> data + algorithm

对于data来说，访问控制（常/变量），数据类型，名，值，地址都很重要

在整个程序中，存储数据的内存分为几个部分

* 数据
  * global data 全程生命周期
  * stack 后进先出 编译器决定生命周期
  * heap 动态数据 生命周期由程序员决定

ADT abstract data type 

离散的存储空间

### 表达式 Expression

* 语义的确定顺序
  * 优先级和结合性 先乘除后加减

* 类型转换约定
  * 默认隐式 coersion
  * 强制 casting 

* 求值次序
  * 取决于compiler

需要利用尽可能清晰的语义来提升程序的可读性 不依赖于compiler 计算时防止overflow 计算前约定/异常处理

操作符可重载 即多态

<font color = "red">问题：难道多态一定属于OO吗</font>

```cpp
x+y;
cout<<8;
//这两个表达式本质是相同的，"<<"是一个双目操作符，与"+"类似
```

我们没有权利创造新的操作符

**赋值表达式**

**左值=右值表达式**

* 左值：可以出现在赋值表达式左部的表达式，具有存放数据的确定地址

* 类型不同时，先计算右值表达式的值，再转换为左值类型，然后赋值

例：

```cpp
double a = 5/2; //先算5/2再转换成double

int& foo(){
    int x = 0;
    return x;//这样的返回形式是不行的，x的生命周期随foo()消失而结束
}
```

**右值表达式**

```c++
int &&x = 1;//给临时的数据起名
```

**算术表达式**

前增量（前减量）    ++a ( --a )     前增量的结果是左值，后增量（后减量）    a++ ( a-- )

提高编译结果的执行效率

**条件运算表达式**

<exp1> ? <exp2> : <exp3>

唯一的三目运算符，只计算一个运算分量，不可重载

如果<exp2> <exp3>值类型相同，且均为左值，则该表达式为左值表达式

​	**就近原则**

```cpp
int b = x > 0 ? 1 : x == 0 ? 0 : -1;
```

**逗号表达式**

<exp1> , <exp2> , <exp3>,...,<expn>

```cpp
int a,b,c,d;
d = (a = 1,b = a + 2,c = b + 3);//相当于做3次，d = a,d = b,d = c,最后输出6
cout<<d;
```

**字位运算符表达式**

~取反,&与,|或,^异或

除了取反是单目的，其他是双目的

<font color = "red">问题：如果想交换两个整数，函数怎么写最简单？</font>

```cpp
a = a ^ b;
b = b ^ a;
a = a ^ b;
```



### 基本数据类型 Data type

built-in data type : char/int/float/double

typedef 为已有的数据类型创建同义词（取个别名）

typedef double profit ;
typedef int INT ;

主要是为了解决移植问题。如有些平台int为16位，定义short位INT16