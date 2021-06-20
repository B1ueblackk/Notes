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

## 结构化程序设计

程序 -> data + algorithm

对于data来说，访问控制（常/变量），数据类型，名，值，地址都很重要

在整个程序中，存储数据的内存分为几个部分

* 数据
  * global data 全程生命周期
  * stack 后进先出 编译器决定生命周期
  * heap 动态数据 生命周期由程序员决定

ADT abstract data type 

离散的存储空间

### 表达式

* 语义的确定顺序
  * 优先级和结合性 先乘除后加减

* 类型转换约定
  * 默认隐式 coersion
  * 强制 casting 

* 求值次序
  * 取决于compiler

需要利用尽可能清晰的语义来提升程序的可读性 不依赖于compiler

计算时防止overflow 计算前约定/异常处理

### 基本数据类型

typedef 为已有的数据类型创建同义词（取个别名）

typedef double profit ;
typedef int INT ;

主要是为了解决移植问题。如有些平台int为16位，定义short位INT16