## const一族🤔

### 1. const限定符

**默认状态下，const对象仅在文件内有效，如果要在多个文件中共享，需要在定义与声明之前都添加extern关键字**

#### const的引用

引用及其对应的对象都应是常量
```cpp
const int i= 20;
const int &ri=i;
```

#### 指针和const

对于常量，必须使用常量指针指向它

#### 顶层和底层const

顶层const表示指针本身是个常量，底层const表示指针指向的对象是一个常量
顶层const可以表示任意的对象是常量

### 2. constexpr变量

将变量声明为**constexpr**类型以便由编译器确定一个变量的值是否是一个常量表达式
这些常量表达式在编译时就会计算出结果
对于字面值类型(算数类型、引用、指针)可以被定义成constexpr
特别地，constexpr类型的指针必须被初始化成0，或者是在**固定地址**的变量
例如函数中的变量，由于不存放在固定地址中，所以**不能**声明成constexpr类型的变量

### constexpr函数

参数、返回值都必须是常量类型，且函数体只有一个return语句
```cpp
constexpr int func(){return 3;}
```


## 类型相关的😺
### 1. 类型别名using

除了typedef外，也可以使用**using I = int;** 这种语句来给类型起名
using的另一种用法：``using namespace::name;``

### 2. auto说明符

auto作为一种类型说明符，可以分析表达式的类型，但是在一条语句中只能声明一种类型的变量

auto会忽略顶层const，保留底层const

### 3. decltype类型指示符

decltype不计算表达式，而是分析其类型

decltype返回的类型包括const和引用