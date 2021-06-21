# c++

### 预处理

```c++
//Source Code -> cPP(preProcess) -> Cfront -> CC -> Object Code
#include
#define
```

### constexpr,auto,decltype

```c++
constexpr int *np = nullptr;//constexpr会添加顶层const
const int i=0,&ci = i;
int *p = &i;
auto b = ci;//此时b是整数,因为auto会忽略顶层const，但不会忽略底层const
decltype(ci) x = i;//x是i的引用，类型是in&
decltype((i))x = i;
decltype(*p) x = i;//以上两种类型都是引用类型
```

### vector

```cpp
#include<vector>
#include<string>
//vector是一个类模版
//vector容纳绝大多数对象，但引用不是对象，所以不能作为vector元素
int n;
vector<T> v1;
vector<T> v2(v1);
vector<T> v2=v1;//两种都是v1的副本
vector<T> v2(n);//n个初始化的T对象
vector<T> v3{a,b,c}//以a,b,c对象初始化v3
vector<T> v3={a,b,c}//与上面等价

vector<string> s1(6,"sample");//用6个sample初始化

s1.push_back("another_sample");//在vector中加入元素
```

### 迭代器

```cpp
//(*iterator).mem <=> iterator=>mem
string s = "sample";

```

### 多维数组

```cpp
int (&row)[4] = a[1];
//无论多少维数组都是连续的
int *ip[4];//指向整形的数组
int (*ip)[4];//指向数组的指针
int a[3][4];
int (*p)[4];
p = &a[2];//p指向a的第三个元素（a的第三个元素是有4个整形的数组）

for( auto p = ai; p != ai + 3; p++){
  //p指向ai的最外层数组，最外层数组大小为3
  for( auto q = *p; q != *p + 4; q++){
    //q指向p数组首项，即p解引用的地址
    cout << *q << endl;
  }
}

//也可以
for( auto &p : ai){
  for ( auto &q : p){
    //只有最内层可以不写引用，此时将会被解析成 *int 不能被遍历
    cout << q << endl;
  }
}

//begin end函数
for( auto p = begin(ai); p != end(ai); p++){
  for( auto q = begin(*p); q != end(*p); q++){
    cout << q << endl;
  }
}
```

#### 指针数组

```cpp
//数组中的元素是指针
char *str[] = {"char","string"};
//main 函数
int main(int num,char*argv[],char*[] env){}
```

#### 可变参数

```cpp
typedef char* var_list;
#define _INTSIZEOF(x) ((sizeof(x) + sizeof(int)- 1)& ~(sizeof(int)-1))
//(x+n-1)/n * n
int printf(const char*,***){}
```

#### 函数指针

```cpp
void (*fp)(int);//定义
void func(int);
fp = func;//赋值
//可以结合表驱动进行更加高效的处理
//比如c#中的委托

//冒泡排序
int CompareTo(const void *elem1, const void *elem2){
  //写
}
void mySort(void *base, unsigned width, unsigned num, int (*compare)(const void *elem1, const void *elem2)){
  char *A = (char*)base;
  char *tmp = (char*)malloc(width);
  for(unsigned i=1; i<num ; i++){
    for(unsigned j = 0; j<i ;j++){
      if(compare(base[i*width],base[j*width])){
        //交换存储空间
      }
    }
  }
}
```

### 多级指针

```cpp
void main(){
  char *p1 = "abc";
  char *p2 = "wtf";
  swap(&p1,&p2);
}
void swap(char **p1,char **p2){
  char *temp = *p1;
  *p1 = *p2;
  *p2 = temp;
}
```

### 引用

```cpp
int &y = x;//给已有的空间的一个别名

void push(Node **first, int value){
  Node temp = new Node();
  temp.value = value;
  if(first == null){
    *first = &temp;
  }else{
    (*first)->next = &temp;
    temp.next = *first;
  }
}
//传入**first太麻烦
//引用可以作为返回值（左值）
int &max(int x[], int num){
  //////
  return x[i];
}
//左值应用
char& operator[](int i){}//等价于 s[i] s为string类型 可以进行s[i] = 'A';
```



### 表达式

表达式要么是左值要么是右值

- 赋值表达式
  - 右值先算出来，在根据左值的对象、内存地址进行转换
  - Int &&x = 1; //这是临时数据的引用
- 算术表达式
  - 增量和减量操作符（前增量结果是左值）
- 条件运算符表达式
  - 三目运算符唯一，只计算一个分量，exp2和exp3类型相同为左值
- 逗号表达式
  - 逗号表达式是左值（if expn 是左值）
- IO语句
  - << >> 可以重载 cout <<  返回的是ostream&
- 

右值: 使用的是它的值(内容), 并忽视"指纹"(这个右值的对象属性, 内存地址什么的)

左值: 使用的是它的内存地址, 此时它的值和内存地址同样重要

右值不能当作左值，左值可以作为右值，此时左值的内存地址、对象属性被忽略，比如&符号作为右值是用的是解引用的地址的值，而不是引用的属性

- 赋值运算要一个左值作为左侧运算对象，结果是一个左值
- 取地址符作用于一个左值对象，返回的是右值（指向这个对象的指针）
- 内置解引用（*）、下标运算符（a[1]），返回左值
- 内置类型、迭代器增减运算符，前置版本结果是左值
- 特殊的decltype，假设p类型*int，那么decltype(\*p)结果就是一个左值，得到&int，decltype(&p)为取地址，得到的是右值，是指向p对象的指针，即int\*\*

#### 运算符

```cpp
//运算提升
bool a = false;
bool b = -a;
//运算情况下true为1,false为-1
```

##### 递增递减运算符

- 后置递增递减运算符优先级高于解引用运算符

```cpp
auto pbeg = v.begin();
while(pbeg! = v.end() && *beg > 0){
  *pbeg++;//于是乎等价于 *(pbeg++)
}
```

##### 成员访问运算符

- ptr->mem <=> (*ptr).mem //解引用优先级比点运算符优先级低

##### 条件运算符

- 可以嵌套

  ```cpp
  finalGrade = (grade > 90) ? "high pass" : (grade < 60) ? "fail" : "pass" ;
  ```

##### sizeof运算符

- 返回运算对象的大小(字节为单位)
- 返回值为constexpr

#### 隐式转换

- 数组转换为指针

  ```cpp
  int a[4] = {1,2,3,4};
  int *p = a;
  ```

- 常量转换

  ```cpp
  int i;
  const int* p = &i;
  const int& j = i;
  //如此一来i就是const
  ```

#### 显式转换

##### 命名的强制转换

- static_cast 只要不包含底层const //static_cast\<Type>(target)

  ```cpp
  void *p = &d;//void可以储存任意类型的指针
  double *d2 = static_cast<double*>(p);
  ```

- const_cast 主要用于传参给非常量参数的函数，且必须保证这个函数不会修改这个引用/指针

  ```cpp
  const char *pc;
  char *p = const_cast<char*>(pc); //通过p写操作是未定义的行为
  *p = "no";//这一步不会报错，pc指向的值发生变化
  
  
  const int a = 1;
  const int* p = &a;
  int *cp = const_cast<int*>(p);
  *cp = 2;
  cout << &a << " " << a << endl;
  cout << p << " "<< *p << endl;
  cout << cp << " " << *cp;
  /*
  0x7ffee755f3fc 1
  0x7ffee755f3fc 2
  0x7ffee755f3fc 2
  */
  
  const char *cp;
  char *q = static_cast<char*>(cp);//错误,底层const不能这么转
  static_cast<string>(cp);//可以，字符串转string
  const_cast<string>(cp);//不行，只改变常量属性
  ```

- reinterpret_cast

  ```cpp
  int *p;
  char *pc = reinterpret_cast<char*>(p);
  
  string s = string(pc);//非常危险，因为pc实际上还是指向int
  ```

但在早期，直接(Type)转换，风险较大

### 语句

#### goto语句

```cpp
begin:
			cout << " " << endl;
if(notPermitted) goto : begin;
```

### 异常

```cpp
while(cin >> item1 >> item2){
  try{
    if(/* */)throw runtime_error("blabla");
  }catch(runtime_error error){
    cout << error.what() << endl;
    cout << "bbbbb" << endl;
  }
}
```



### 函数

可读不可运行

可写不可运行

- code r

- data wr

- stack wr

- heap wr

##### Runtime Environment

•均是契约，由**complier**和 **linker**共同管理。
 • **__cdecl** •**__stdcall** •**__fastcall**

- cdecl 调用者决定返回个数 可变参数
- stdcall 被调用者返回
- fastcall 参数优先放寄存器

c函数声明时 在前面加 extend "C"

##### 默认参数

默认参数放在原型

#### inline函数

- 将函数直接映射成代码，不用funtion call
- inline的实现必须全部放到cpp里（每个需要用到inline的cpp）
- 可以优雅地写在一个 inline.h头文件里，然后include，但实际调用没有改变，相当于编译器copypaste
- 非递归，没有函数指针（不是函数）
- 最终是否是inline取决于compiler，compiler一个cpp一个cpp编译，发现一个cpp inline不能inline时，其他cpp不会得到通知，照样编译
- 使用频率高（程序局部性原理）、简单（不要用switch loop，可能造成段页式抖动[病态的换页]，复杂指的是指令的跳转多少）、小段代码

### Struct

```cpp
struct name{
int a;
char b;
}
//默认public，罗列参数
```

### 类

```cpp

```



### union

```cpp
union B
{
  char b; //1
  int a; //4
  short c; //2
}
//共享存储空间，支持多态

/*
例：matrix 
double element[3][3]
*/
union Matrix{
  struct
  {
    double a11,a12,a13;
    double a21,a22,a23;
    double a31,a32,a33;
  };
  double element[3][3];
  //element 和struct共享存储空间，他们的内存排列相同
}
union FIGURE{
  struct line{
    FIGTYPE type;
    int x1,y1,x2,y2;};
  struct rec{
    FIGTYPE type;
    int x,y,r;};
  struct eclipse{
    FIGTYPE type;
    int lef,top,rig,bot;};
  union FIGURE{
    FIGTYPE t;
    line line;
    rec rec;
    ecl ecl;
  }
}
void input(FIGURE figure,int num){
  char type;
  FIGURE figure;
  for(int i=0; i<num ; i++){
    std::cin >> figure;
  }
}
void draw(FIGURE figure){
  swich(figure.t){
    case(LINE): draw
      ////////
  }
}
```

### 指针

> ```cpp
> typedef int* Pointer;
> #define NULL 0
> void func(int);
> void func(char);
> func(NULL);//二义性
> func(nullptr);//目前指针初始化 用nullptr
> int a;
> int *p = &a;
> p = p + 1;// p = p + sizeof(int);等价
> char* str = "AB";
> cout << str;//打印 AB 因为 <<(ostream& o, char*) 被重载
> cout << (char*)str;//打印str地址
> 
> 
> void memset(void *pointer, unsigned size){
>   char *p = (*char)pointer;
>   for(int k = 0; k < size ; k++){
>     *p++ = 0;
>   }
> }
> void showBytes(void *p, unsigned n);
> //compiler 会把常量属性替换为常量
> 
> ```

#### new

```cpp
int *p = new int[]; //
delete p;
int *q =new int[8];
delete[] q;//逐个调用析构函数

int *m = (int*)melloc(sizeof(int));
int *n = (int*)melloc(sizeof(int) * 8);//用额外的空间储存空间申请的大小
free(m);
free(n);//访问额外申请的大小
//有一个问题free 和delete[] 不会确认是否是申请的第一个地址,所以要写一个指针专门指向被申请空间的第一个地址
```



### namespace

```cpp
namespace Your_Code{}
namespace YC = Your_Code

```

### 编译预处理

- 潜伏于环境
- 穿透作用域

```cpp
#include
#define
/*
定义常量：
#define Pi 3.14 //const

宏： 
#define MAX(x,y) ((x)>(y) ? (x):(y)) //可以用内联函数替换

subroutine
types
可用泛型替代：//编译器在需要的时候生成需要类型的函数

renaming
#define Func(x,y) x##y // ##表示连接connection
#define ToString(x) #x //给x加双引号
#define ToChar(x) #@x //给x加单引号


*/
#ifdef
/*
版本控制
#ifdef MY_PRINTF_VERSION
	#define MY_PRINTF_VERSION 1
#endif
#if MY_PRINTF_VERSION == 1
void func_1(){}
#if MY_PRINTF_VERSION == 2
void func_2(){}
*/

#pragma
//warning
```



### Structure+Algorithm

#### Data

```cpp
//class NAME = VALUE;

// 内存中数据分布
//	DATA
//  ||
//  CODE
//  ||
//  STACK
//  ||
//  HEAP
```

ADT decide the domain of data and it's algorithm. 

Compiler + Linker(文件头".h")

```cpp
typedef int Whatever;//别名
//类型转换精度
//double转int compiler会给warning
//int 转 float 可能损失精度 float 23+8 int 32 25位1以上就会丢失精度，long long 转 double 也一样
```

##### 操作符可重载

- 操作符运算结构不改变
- 短路表达式一般不要重载，因为会重载为函数

### 案例

#### Switch

```cpp
enum Target{
  blabla = 0,
  babla = 3
};
// 内存中table形式 index(每个枚举的index) - codeAddr 
// table从0开始 到最大的枚举index 且连续
/*
mov 
cmp 检验是否越界（table）
**offset
mov 取代码地址
jmp
*/
switch(i):
{
  //可用表驱动增强性能
  case blabla:
  //case enum类型
		cout << "first case" << endl; break;
  case babla:
  //cmp
  	cout << "second case" << endl; break;
  default:
  	break;
}

```

### OO

c向c++的过渡，c++中struct和class没有本质区别。

cfront编译之下只在源代码层面实现封装，实际编译出来没有OO

#### 封装（基于对象开发）

##### Class

在头文件中声明成员变量和成员方法(不需要参数名)

在源文件中定义成员函数

```cpp
//.h 在编译过程中check参数
//如果在头文件中定义方法，则这个方法是建议inline function 比如setter和getter
class TDate{
  public:
  void setDate(int,int,int);
  private:
  int year,month;
};
//.cpp
void TDate::TDate(int y,int m, int d){
  //定义方法
}


TDate g;//这是一个初始化对象的方法，在全局静态区
int main(){
  TDate t;//栈上初始化，比堆块，自动析构
  TDate *g = new TDate;//堆上初始化
}
//c++对象初始化方法有三种，java只能在堆里创建，返回一个引用
```

##### 构造函数

没有返回类型，不能外部调用，可以重载

```cpp
//对对象进行初始化
//为对象创建标识符、开辟内存空间、初始化
public:
	A();
	A(int);
	A(char*);

A a1=1;//只有一个参数，作为一个参数传进构造函数
A a2=A("abc"); A a3("abc"); A a4="abc";
A a4[4]; //调用a4[0], a4[1], a4[2], a4[3]的A()

//成员初始化表
int x;
const int y;
int& z;
public:
	A():y(1),z(x),x(0){x=100;}//按照声明次序执行初始方法表，且先于构造函数体 比如这个就是先执行x，再y，再z
//只有static const 能够在声明的时候赋值
//非静态变量都可以在类中赋值(c++11)

class A{
  int m;
  public:
  	A(){m=0;}
  	A(int m1){m=m1;}
}
class B{
  int x
  A a;
  public:
  	B(){x=0;}
  	B(int x1){x = x1;}
  	B(int x1, int m1):a(m1){x=x1;}//对对象也用成员初始化表进行初始化
       }

//拷贝构造函数 自动调用
A a;
A b = a;//调用构造函数A b(a);
A a, b=a;//赋值

f(A a){}//调用时调用拷贝构造函数
f(){A a; return a;}//返回拷贝

//定义拷贝构造函数
public:
	A(const A&);//不能用A(const A a);这相当于传参的时候调用拷贝构造函数，等于递归
//如果成员中有指针指向一段堆内存，默认拷贝构造函数会拷贝指针，但指向同一块内存，delete原来的内存时，现在拷贝的结果也被delete，但也同时有访问权限，这种指针就叫悬挂指针，这时需要自定义拷贝堆内存
	
```

##### 转移构造函数

```cpp
//应用场景
string generate(){
  return string("test");
}
string S = generate();//要拷贝三次
//可以把左值赋给一个引用，因为引用是指针的别名，但可以把右值赋给一个const的引用
//move constructor
//A(A&&) int &&x = 6;//相当于给右值一个引用
string::string(string &&s):p(s.p){s.p = nullptr;}//移动构造函数
//这样的话 string S = generate();就不需要拷贝，而调用移动构造函数
```

##### 析构函数

~<类名>()

对象内存退回时调用:方法结束(stack)，delete(heap)//释放资源（除对象内存之外的网络链接、文件等等）

RAII(Resource Acquisition is Initialization) 对象生命周期和资源绑定

```cpp
//一般情况是public 可定义为private
class A{
  public:
  	A();
  	void destroy(){delete this;}
  private:
  	~A();
  //这样就只能在堆上创建对象（限制）
}

//类中声明了堆中对象 析构函数需要将其释放(delete[])
//一般new对应一个delete
```

##### 动态对象

```cpp
A *p;
p = new A;
//在程序中申请sizeof(A)的内存（可重载）,用默认构造函数初始化,返回这段内存的指针（地址）
delete p;
//首相调用析构函数,然后释放对象空间（可重载）
//primitive变量也可以在堆上初始化，但是不用调用构造函数
```

##### 动态对象数组

```cpp
A *p;
p = new A[100];//前提是A有默认构造函数A();
//c++11 
p = new A[100]{0};//全部初始化为0
p = new A[100]();//默认全部初始化为0
delete[] p;//[]不能省 delete[]会从p[0]-4的内存开始释放(内存头)

//二维
char** array;
array = new char*[rows];
//对每一个char* 单独分配
//delete[] 每一个char*

//或者用一维数组，还可以节约空间
```

##### const成员

```cpp
class A{
  int x;
  int y;
  int& ref;
  public:
  	A(int a,int b):x(a),y(b),ref(*new int);
  	~A();
  	void f(){x++;y++;}//f(A* const this);
  	void show() const{std::cout << x << " " << y<< std::endl;}//show(const A* const this);
  	void increment() const{ref++;}//ref甚至可以指向x,y 都可以通过编译
  	//const_cast<A*>(this)->x=1;
}
const A a(0,0);
a.f();//错误
a.show();
```

##### static

```cpp
//.h
class A{
  static int shared;
}
//.cpp
A::shared = 0;
//singleton
class singleton{
  protected:
  	singleton(){}
  	singleton(){const singleton&};
  public:
  	static singleton Instance(){
      return instance == NULL ? instance = new singleton:instance;
    }
  	static destroy(){delete instance; instance = NULL;}
  private:
  	static singleton* instance;
}

```

##### friend

```cpp
class C{
  f();
}//class C;
void func();//extern void func();
class A{
  friend void func();
  friend class C;//friend C; friend class C;可以没有声明,此时会创建一个 class C;
  friend void C::f();
  //必须已声明
}

class Vector;//前项声明
class Matrix{
  friend void multiply(Matrix&,Vector&, Vector&);//不能传对象，因为Vector没有定义,没办法在值传递时分配空间
  //在cpp中include Vector 也可以写在Vector的定义后面
}

#include "Matrix.h"
class Vector{
  friend void multiply(Matrix&,Vector&, Vector&);//只能在cpp文件中定义
}

//////
class Base{
  protected:
  	int prot_mem;
}
class Sneaky:public Base{
  friend void clobber(Sneaky&);
  friend void clobber(Base&);
}
void clobber(Base& base){
  prot_mem = 0;//错误，不能访问Base的成员，友元没有传递性
}
```

#### Inheritance 

private被子类继承，但对子类不可见

```cpp
class Student{
  int id;//private
  public:
  	char nickname[16];
		void set_ID(int x) { id = x; }
		void SetNickName(char *s) { strcpy(nickname,s);} 
  	virtual void showInfo(){ cout << nickname << “ : “ << id <<endl; }
}
class Undergraduated_Student : public Student//不能访问 Student 的私有成员，因为类也是对象
  //private继承 所有protected, public 变为private
  //protected继承 public变为protected
{
  int dept_no; 
  public:
		void setDeptNo(int x) { dept_np = x; }
		void set_ID(int x) {......}
  	using Student::showInfo;//引入基类命名空间，防止重载覆盖
		void showInfo()
				{ cout << dept_no << “: “<< nickname << “ : “ << id <<endl; }//编译的时候确定调用版本
  	//void showInfo(char* s){...} 会把基类同名函数覆盖(定义之后)
  	//调用步骤:查找名称(namespace),找到void showInfo(char* s) => 参数匹配
  
	private:
		Student::nickname; //改变基类成员访问权限
  	void SetNickName ();
}
//拷贝构造函数
B(const B& b):A(b){}//调用A的拷贝构造，如果不跟A(b),则默认拷贝时调用A的默认构造函数
public A::A;//继承A的构造函数(命名空间)，方便赋值

```

##### 类型相容

```cpp
class A{}
class B:public A{}
A a;
B b;
a = b;//强制类型转换(对象切片，派生类属性不会给a)，派生类属性丢失(b和a分配的内存大小不一样)
B* pb = &b;
A* pa = pb;//可以，无对象拷贝
A& ra = b;//可以，无对象拷贝
```

##### 绑定

```cpp
class A
{ int x,y;
public: void f();
};
class B: public A { int z; public:
void f();
void g(); };

func1(A& a) {... a.f(); ...}
func2(A *pa)
{ ... pa->f(); ...}
func1(b); //A 
func2(&b); //error
//静态绑定（根据对象静态类型），在编译时确定了类型函数
//cpp默认前期绑定
//用virtual指定后期绑定
//析构函数往往是虚函数
A& a = b;
delete a;//如果不是虚函数，资源泄露
```

##### 后期绑定

```cpp
//为每一个有虚函数的类创建一个虚函数表
//有覆盖就采用新函数的地址，否则就是原来的地址
//对象头（实例上方）有虚函数表的指针
class A
{ 
  int x,y; 
  public:
		virtual f(); 
		virtual g(); 
  	h();
};
 class B: public A { 
   int z; 
   public:
		f(); 
   	h();
};
A a; 
B b; 
A *p;
//调用函数
(**((char *)p-4))(p)//(char *)转换决定按字节移动,解引用分别取虚函数地址，实际函数，传参p(this)
//注意：在构造函数调用结束后，类型才真正决定
//例：
class A{
  public:
  A{f();}
  virtual void f();
  void g();
  void h(){f();g();}
}
class B:public A{
  public:
  	void f();
  	void g();
}
B b;//调用A::f();
A *pa = &b;
pa->g();//A::g();
pa->h();//B::f(); A::g();

//析构函数也要用virtual，不然会导致资源释放不完全
class B{}
class mystring{}
class D:public B{
  mystring name;
}
B* p = new D;
```

![虚函数表](/Users/lunajohn/Library/Application Support/typora-user-images/image-20210519105922398.png)

##### override

```cpp
//override必须要返回值和参数都一样，重载函数不需要返回值相同
struct B{
  virtual void f1(int) const;
  virtual void f2();
  void f3;
}
struct D:B{
  void f1(int) const override;
}
//纯虚函数
class A{
  public:
  	virtual void f()=0;//纯虚，不能内联定义，派生类不能继承方法体
  //这个类就为抽象类，不实例成对象
  //派生类必须给出实现，也可以显式调用虚方法（如果定义实现了的话）
  //纯虚函数在虚方法表中不给地址，只预留一个地址的位置
  //可以避免对象切片
}

//用法
AbstractFactory *fac;
case Win:
	fac = new WinFactory;
case Mac:
	fac = new MacFactory;
```

##### 什么是好的继承

> 李氏替换法则(LSP)

* 子类相对于父类前置条件更少后置条件更多

* 外部调用只需要知道父类的接口

##### 私有继承

> 公有的接口都变成private了，不复用父类的代码

* 需要使用protected成员，或重载virtual function

* 用基类来实现派生类，has_a 关系
* 在设计方面没有意义，只在实现上有意义
* 可以省内存，私有继承外部包

> 私有继承后，子类型不能默认地转换为基类型

###### 缺省参数

```cpp
class A{
  public:
  	virtual void f(int x=0)=0;
};
class B{
  public:
  	virtual void f(int x=1){
    	cout << x;
  }
}
class C:public A{
  public:
  	virtual void f(int x){
      cout<<x;
    }
}
//都输出0，函数地址是虚函数表查到的，函数参数是静态绑定的
```

#### 多继承

* Class \<name>:[<继承方式>]<基类名>,[<继承方式>]<基类名>,...
* {成员}

> 可能有重名方法

<center>    <img style="border-radius: 0.3125em;    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);"     src="/Users/lunajohn/Library/Application Support/typora-user-images/image-20210521154613382.png">    <br>    <div style="color:orange; border-bottom: 1px solid #d9d9d9;    display: inline-block;    color: #999;    padding: 2px;">多继承方法重名</div> </center>

<center>    <img style="border-radius: 0.3125em;    box-shadow: 0 2px 4px 0 rgba(34,36,38,.12),0 2px 10px 0 rgba(34,36,38,.08);"     src="/Users/lunajohn/Library/Application Support/typora-user-images/image-20210521155029126.png">    <br>    <div style="color:orange; border-bottom: 1px solid #d9d9d9;    display: inline-block;    color: #999;    padding: 2px;">格设计</div> </center>

- 声明次序决定基类构造函数和析构函数调用次序

- Class D: B,C{} 调用构造函数ABACD
- 也可以用虚继承

> Class A;
>
> class B: virtual public A;
>
> class C: public virtual A;
>
> class D: B,C;

可以在D构造时调用A，然后再构造B,C

若A中有virtual方法且被B,C重载, D可以直接重载B,C

- 假如一个类继承了多个含虚方法的类，那么内存中会有所有继承对象的方法表

### 多态

#### 重载

操作符重载

```cpp
//事例
enum Day{
	SUN, MON, TUE, WED, THU, FRI, SAT
};
Day operator++(Day& d){
  return (d==SAT)?SUN : Day(d+1);
  //尽量返回new的对象
}
ostream& operator << (ostream& o, Day& d){
  switch (d)
	{	case SUN: o << "SUN" << endl;break; 
   	case MON: o << "MON" << endl;break; 
   	case TUE: o << "TUE" << endl;break; 		
   	case WED: o << "WED" << endl;break; 
   	case THU: o << "THU" << endl;break; 
   	case FRI: o << "FRI" << endl;break; 
   	case SAT: o << "SAT" << endl;break;
	}
	return o;//这个没办法
}

class A
{ intx;
public:
	A(int i):x(i){} 
 	void f() { ... } 
 	void g() { ... }
};

void(A::*p_f)();
//p_f是A的成员，指向一个void方法
p_f= &A::f;
(a.*p_f)();//调用，重载了谁看得懂
//.*对象访问操作符，访问对象方法
//尽量不要重载 . .* :: ?:
//::不具备重载的特征，因为它作用在名称上，不作用在变量上
//?:重载会把它变成函数，会在传参时计算:两端的值，效果大不同

//全局函数重载
friend ty operator# (<arg1>,<arg2>);//对象内
//= () [] -> 不作为全局函数重载
//后三个是为了保证先对象后操作符的顺序
//= 是因为类内有默认拷贝构造，全局重载等于没重载

//例 重载乘法
class Rational { 
  public :
		Rational(int,int); private:
		int n, d;
		const Rational& operator *(const Rational& r);
};
//Rational *result = new Rational(n*r.n,d*r.d);
//在return语句里构造对象，编译器会自动调用移动拷贝构造函数，不会调用拷贝构造

//return *result; x*y*z x*y的对象内存泄露

//static Rational result;
//result.n = n*r.n; result.d = d*r.d;return result;
//同时进行乘法操作 (a*b) == (c*d)
```

#### 单目操作符重载

```cpp
class Counter{
  int value;
  public:
  Counter(){value = 0;}
  Counter& operator ++(){value++; return *this;}//返回左值
  Counter& operator ++(int){Counter temp=*this; value++; return temp;}//返回右值
}
```

##### 赋值操作运算符重载

- 默认
  - 逐个成员赋值
  - 成员含有对象，定义递归
- 不能继承

```cpp
A& operator= (A& a){
  
  return *this;
}
class A{
  public:
  	ObjectID identity() const;
}
//返回非const引用，用于支持(a=b).f()
//避免自我赋值，this = &rhs; p1->identity() = p2->identity();
//防止内存分配失败出现悬垂指针
```

##### 下标操作符重载

```cpp
class string
  {
	char *p; 
public:
	string(char *p1){ 
    p = new char [strlen(p1)+1]; strcpy(p,p1); 
  }
//1
  char& operator [](int i) const { return p[i]; }
//2.
  const char operator [] (int i) const { return p[i]; }
	virtual ~string() { delete[] p;} 
};
string s("abcd"); //这个版本精确匹配1
const string cs("const"); //精确匹配2, 只能调用常成员函数
```

#### 特殊操作符

```cpp
//没有[][]或者[][][]操作符
class Array2D
{ 
  int n1, n2; 
  int *p;
public:
Array2D(int l, int c):n1(l),n2(c){ p = new int[n1*n2]; }
virtual ~Array2D() { delete[] p; } };
int & Array2D::getElem(int i, int j) { ... }

//重载[] 假设Array2D a(2,2);
int *operator[](1)(int);//重载 a[?] 返回int* 指针偏移按int大小，所以二维不用重载，也不能重载（因为返回的是int*)，所以多维情况下，高维数组返回的应该是下层代理类的对象(&)

//方案2 代理类/内部类
class Array1D 
{ int *q ;
	int& operator[](j) { return q[j]; }
}


class Array2D { 
public:
	class Array1D { 
  public:
    explicit Array1D(int *p) { this->p = p; }//explicit强调必须显示调用构造函数，不能通过return单参数隐式构造函数
    int& operator[ ] (int index) { return p[index]; }
    const int operator[ ] (int index) const { return p[index]; }
	private: 
    int *p;
	};
	Array2D(int n1, int n2) { p = new int[n1*n2]; num1 = n1; num2 = n2; } 
  virtual ~Array2D( ) { delete [ ] p; }
	Array1D operator[] (int index) { return p+index*num2; }//这里使用了隐式构造函数的特性：对单参数类有效
 	const Array1D operator[] (int index) const { return p+index*num2; } 
private:
	int *p;
	int num1, num2; };

//()操作符：函数调用，类型转换
class Func{
  double para;
  int lowerBound,upperBound;
  public:
  	double operator()(double,int,int);
}
Func f;//函数对象
f(2.4,0,8);
//用途，比如在之前的Array2D类中重载()，可以不用委托类

class Rational {
  public :
Rational(int n1, int n2) { n = n1; d = n2; } 
  operator double(){return (double)n/d;}//这个时候double()被重载,一部分原因是为了与函数调用重载区分,同时也确定了被转换类型
private:
int n, d;
};
//使用
Rational r(1,2); double x=r; x=x+r;//隐式类型转换


//-> 指针间接引用操作符
a.orperator->()->f()//a.orperator->()返回一个指针或者重载过()的对象
  
class Cpen{
  int color;
  int width;
  public:
  	void setColor(int c){color = c;}
};
class Cpanel{
  Cpen pen;
  int m_bkColor;
  public:
  	Cpen* operator->()(return &pen;)
}
//用途2 封装控制内存资源
void test(){
  AWrapper p(new A);//用栈对象管理堆对象
  p->f();
  ///
}
<typedef class T>
class AWrapper{
  T p;
  public:
  	AWrapper(A *p){this->p = p;}
  	~AWrapper(){delete p;}//在封装类消亡之后，堆上p消亡
  	A*operator->(){return p;}
}

```

##### new、delete操作符重载

- 频繁调用操作系统内存管理，影响效率
- 程序自身管理内存，提高效率
  - 调用一次系统内存分配，自己管理
  - 针对这块内存自己管理，用重载new/delete实现
  - 重载new只是提供了一个优先级较高的new，并没有重载全局new
  - 肯定都是（类）静态成员函数，且可继承

实操

```cpp
void* operator new(size_t size,...)//可选参数，可以重载多个new
 //第一个size是系统自己计算的，计算后给size赋值
 //可以在重载之后可以在栈上new对象，即可以重复利用栈空间，然后显示调用析构函数
A* p = new ()A;//
```

delete

```cpp
//delete只能重载一次
void operator delete(void *p,size_t size)
  //size同new，如果虚析构函数被重载，可以判断是否是基类
```

### 类属多态

- 类型参数运用
  - 宏：可以实现简单功能，但是没有类型检查
  - 函数重载：要定义很多重载函数
  - 函数指针：要定义很多参数，全都是指针操作，实现复杂，可读性差

##### 函数模版

```cpp
template <typename T>
void sort(T[], unsigned int num){
  for(...)
}
//可以带普通参数
template <class T,int size>
  void f(T a){}
f<int,10>(1);//显示调用

template<> f<int>()

template <class T>
class Stack
{
  T buffer[100]; 
public:
  void push(T x);
  T pop(); };
template <class T>
void Stack <T>::push(T x) { ... }
template <class T>
T Stack <T>::pop() { ... }

Stack <int> st1;//显示实例化
Stack <double> st2;

//带参数的类模版
template <class T, int size> class Stack
{ 
  T buffer[size];
public:
	void push(T x); 
  T pop();
};

template <class T, int size>
void Stack <T, size>::push(T x) { ... }
template <class T, int size>
T Stack <T, size>::pop() { ... }
......
Stack <int, 100> st1; 
Stack <double, 200> st2;
```

- 如果在模块A中要使用模块B中定义的某模板的实例，而在模块B中

  未使用这个实例，则模块A无法使用这个实例

```cpp
//file1.h
template <class T> class S
{ 
  T a;
public: void f();
};

#include "file1.h"
template <class T> 
  void S<T>::f() { ...}
template <class T> 
  T max(T x, T y){ return x>y?x:y;}
void main() 
{ 
  int a,b;
	max(a,b); 
 	S<int> x; 
 	x.f();
  }

//file2
#include "file1.h"
extern double max(double, double);
void sub(){
  max(1.1,2.2);//错误，f1.h中没有声明max
  S<float> x;//错误，没有S<float>的定义
  x.f();
}
//所以建议模版声明和定义都放在头文件中

//神奇的用法
template<int N> class Fib
{ public:
enum { value = Fib<N - 1>::value + Fib<N - 2>::value };
 //模版源编程理论上和函数一样
};
  
template<> 
class Fib<0> //显式匹配
{ 
public:
	enum { value = 1 }; 
};

template<> 
class Fib<1>
{ 
public:
enum { value = 1 };
};
void main(){
  cout << Fib<8>::value << endl;//编译时完成
}
```

### 异常

- 可以预见
- 无法避免

```cpp
throw 1;
catch(int){}
//throw 类型要调用拷贝构造，所以需要知道构造函数的定义
//尽量throw对象
//catch类型完全匹配

class FileErrors{};
class NonExist:public FileErrors{};
class DiskSeekError:public FileErrors{};

int f(){
  try{
    WrongFormat wf;throw wf;
  }
  catch(NonExits&){}
  catch(DiskSeekError&){}
  //子类到基类
}

//例题
class MyExceptionBase { };
class MyExceptionDerived: public MyExceptionBase { };
void f(MyExceptionBase& e) { 
  throw e;
  //这步会将传进来的对象转换成父类（拷贝构造，对象切片，编译决定）
}
int main() { 
  MyExceptionDerived e;
try {
	f(e);
} catch (MyExceptionDerived& e) {
cout << "MyExceptionDerived" << endl;
} catch (MyExceptionBase& e) {
cout << "MyExceptionBase" << endl;
} }

catch(int){throw;}//重抛
catch(...){}//捕获全部异常

class A{
  
}

template<class T,class E>
  inline void Assert(T exp, E e){
  if(Debug)
    if(!exp)throw e;
}
//在class中对构造函数异常处理
TryCatchTest()try:a(1),b(2){
        throw 1;
    }
    catch(...){
        cout << "shit"<<endl;
    }

void processAdoptions(istream& dataSource){
  while(dataSource){
    ALA *pa = readALA(dataSource);
try
{ pa->processAdoption();}
  catch (...){ 
		delete pa;
    throw;}
  delete pa;//代码重复，可以用智能指针思想
  }
}
//
template<class T>
  class auto_ptr{ 
public:
			auto_ptr(T *p=0):ptr(p) {} 
     	~auto_ptr() { delete ptr; }
			T*operator->() const{returnptr;} 
      T& operator *() const { return *ptr; }
private: 
      T* ptr;
  }//在栈上创建auto_ptr,自动析构
  
void processAdoptions(istream& dataSource) { while (dataSource)
{ 
 auto_ptr<ALA> pa(readALA(dataSource)); 
 pa->processAdoption();
} }

//另一个例子
void display(const Information& info){
  WINDOW_HANDLE w(createWindow());
  destroyWindows(w);//转换为WINDOW_HANDLE类型
}
class WindowHandle{
  public:
  WindowHandle(WINDOW_HANDLE handler):w(handler){}
  ~WindowHandle(){destroyWindow(w)};
  operator WINDOW_HANDLE(){return w;}
  //重载类型转换操作符
  private:
  	WINDOW_HANDLE w;
  	WindowHandle(const WindowHandle&);
  	WindowHandle& operater= (const WindowHandle&);
}
  
```

### IO

- 全局友元函数重载 << >>
- 如果有继承打印方法，用非虚接口解决问题，即用全局重载函数调用虚函数

```cpp
class CPoint2D
  {
double x, y; 
  public:
//...
virtual void display(ostream& out)
{ out<<x<<‘,’<<y<<endl; }
};

ostream& operator << (ostream& out, CPoint2D &a) { a.display(out); return out; }

class CPoint3D: public CPoint2D
{
double z; public:
//...
void display(ostream& out)
{ CPoint2D::display(); out << ‘,’<< z << endl; } };
```

### 虚化构造函数

```cpp
class NLComponent {...};
class TextBlock : public NLComponent {...}; 
class Graphic : public NLComponent {...}; 
class NewsLetter
{ public:
NewsLetter(istream& str)
{ while (str) components.push_back(readComponent(str)); } static NLComponent * readComponent(istream& str);
private:
list<NLComponent *> components;
  }
NewsLetter(const NewsLetter& rhs)
{ for(list<NLComponent*>::iteratorit=rhs.component.begin(); it != rhs.component.end(); ++it )
 }
component.push_back( ??? );


virtualNLComponent*clone()const=0;
 virtual TextBlock * clone() const
{ return new TextBlock(*this); }
 virtual Graphic * clone() const { returnnewGraphic(*this);}
 NewsLetter::NewsLetter( const NewsLetter& rhs)
{ for ( 
  list<NLComponent *>::iterator it=rhs.component.begin();
it != rhs.component.end(); ++it ) component.push_back((*it)->clone());}


//问题：继承对象大小不一致导致[]指针位移大小不一致，所以最好含对象的数组只存指针
```

### c++11

---

Extern Templates

```cpp
template <class T>
  void myFunc(T t){}

#include"myfunc.h"
int foo(int a){
myfunc(1);//这里实例化了模版
return 1; }

//main
#include "myfunc.h“
/*Tell compiler: this instance has been instantiated in another module!*/ 
extern template void myfunc<int>(int);//告诉main这个模版已经定义了，不让编译器重定义
int main() {
myfunc(1); //这里也实例化了模版，模版重定义报错
}
```

constexpr

- 用于返回常量表达式给switchcase用

```cpp
enum Flags { GOOD=0, FAIL=1, BAD=2, EOF=3 };

constexpr int operator| (Flags f1, Flags f2) { 
  return Flags(int(f1)|int(f2)); //常量表达式
}

void f(Flags x) { switch (x) {
case BAD: /* ... */break;
case EOF: /* ... */ break; 
case BAD|EOF: /* ... */ break; //这里没有重载就会被当成函数，不能确定是不是常量，编译不通过
default: /* ... */ break;
//eOrKronr.orweturn value is not const
} }

void f(Flags x) { 
  switch (x) {
		case bad_c(): /* ... */break; 
    case eof_c(): /* ... */ break; 
    case be_c(): /* ... */ break; 
    default: /* ... */ break;
} }
```

#### lambda表达式

- 是一个函数对象
- 重载模版形成

```cpp
std::function<bool(int,int)>f1(cmpInt);//cmpInt是一个函数指针，指向比较函数

std::function<bool(int,int)>f2([](int a,int b){return a < b;})
  
std::sort(items.start(),items.end(),[](int a,int b){return a < b;});


int main(){
vector<string> vec = {"www.baidu.com", "www.kernel.org", "www.google.com"}; string pattern = ".com";
vector<string> filterd = str_filter(vec,
[&](string &str) {/* []中的表示的是传参过程中参数的访问方式*/
if (str.find(pattern) != string::npos)return true; 
  return false;
  //这个lambda中只需要pattern，所以以引用的方式访问pattern
}); }
  
```

- []不匹配
- [&]所有传参按照引用传递
- [=]所有用拷贝传参
- [=,&foo]除了foo用引用之外，其他用拷贝
- [bar]只拷贝bar

#### 委托构造函数

```cpp

class X { int a;
public:
X(int x) { if (0<x && x<=max) a=x; else throw bad_X(x); } X() :X(42) { }
// ...
};

//采用X(int x = 42) 不等价，X(intx=42)不是默认构造函数
```

#### 初始化

自定义类型数组初始化

```cpp
vector<int> ve = {1,2,3};
//把{}自动翻译成initializer_list<int>
template class vector<T> { //..
vector(initializer_list<T> list) {
for (auto it = list.begin(); it != list.end(); ++it)
push_back(*it); }
};

class A{
int x, y, z;
//Default generated by compiler A(initializer_list<int> list) 
  {
		auto it = list.begin(); 
    x = *it++;
		y = *it++;
		z = *it;
	} 
};
```



问题：

表达式中运算符的优先级

初始化参数列表特性

