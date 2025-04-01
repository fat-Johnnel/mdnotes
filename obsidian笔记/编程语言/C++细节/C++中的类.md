## 定义抽象数据类型

### 定义成员函数

成员函数可以定义在对象内部，也可以定义在外部，定义在内部时是一种隐性的内联函数

### this&const

this是一种隐式的参数，类型是常量指针，指向对象自身

亦即this 的类型是object * const，但如果对象被声明为常量的时候，由于this不是const object * const类型，成员函数无法调用成员变量

解决办法是将函数声明为常量成员函数，声明方法为**在参数列表后加const，这样就可以把this声明为指向常量的指针**

## 访问控制与封装

### 访问说明符：public 和private

public说明在整个程序内可以被访问

private说明只有成员函数可以访问

默认情况下，使用class关键字定义的类成员都是private

### 友元

类可以允许其他类或者函数访问他的飞公有成员，方法是令其他类或者函数成为其**友元**。

声明方式是在类内的声明语句前加一个friend关键字，其作用仅仅是指定了访问权限

## 类的其他特性

### 定义一个类成员

类还可以定义某种类型在类中的别名，和其他成员一样存在访问限制

一个类的定义如下：

```cpp

class Screen{
    
public:
    typedef string::size_type pos;
    Screen()=default;
    Screen(pos ht,pos wd,char c):height(ht),width(wd),contents(ht * wd, c){}
    char get() const {return contents[cursor];}
    inline char get(pos ht,pos wd) const;
    Screen &move (pos r,pos c);
    
    
private:
    pos cursor = 0;
    pos height = 0, width = 0;
    string contents;
};
```

### 可变数据成员

在const成员函数内如果想改变一个成员的值，可以在变量声明时加入mutable关键词，此时定义的成员变量不受const的限制

## 构造函数

### 构造函数初始值列表

通过这种方式初始化和通过构造函数函数体初始化的区别是前者直接进行初始化，后者相当于再赋值

### 初始化顺序

成员的初始化顺序与在类中的定义顺序相同

### 委托构造函数

顾名思义，即将初始化的过程委托给其他构造函数，用法如下所示：

```cpp
class Sales_data{
public:
	//非委托构造函数使用对应的实参初始化成员
	Sales_data(std::string s,unsigned cnt,double price):
		bookNo(s), units_sold(cnt), revenue(cnt *price) {}
	//其余构造函数全部委托给另一个构造函数
	Sales_data(): Sales_data ("",0,0) {}
	Sales_data(std::string s): Sales_data(s,0 ,0){}
	Sales_data(std::istream &is) : Sales_data() {read(is,*this);}
}
```

### 隐式的类型转换

前提：存在只有一个参数的构造函数(也叫做转换构造函数)

示例：

```cpp

class myclass{
    public:
    int a;
    string c;
    myclass(string b):c(b){a=0;}
};

int l(myclass c){
    return c.a;
}
int main(){
    string s = "hello";
    cout<<l(s)<<endl;
    return 0;
}
```

只允许进行一步这种转换

抑制这种转换的方法：在构造函数的声明前加**exlicit**关键字

### 静态成员
| **特性**      | ​**静态数据成员**                                                       | ​**静态成员函数**                                                |
| ----------- | ----------------------------------------------------------------- | ---------------------------------------------------------- |
| ​**定义方式**   | 类内声明，类外初始化（`static int var;`）  <br>（C++11支持`constexpr`类内初始化）      | 类内声明并定义或类外定义（`static void func();`）  <br>类外定义时无需重复`static` |
| ​**内存分配**   | 全局数据区存储，程序启动时分配，结束时销毁                                             | 代码区存储，无独立内存空间                                              |
| ​**初始化要求**  | 必须在类外显式初始化（`int Class::var=0;`）  <br>例外：`constexpr`静态成员可类内初始化<br> | 无需初始化，但只能访问静态成员                                            |
| ​**访问方式**   | `类名::变量名`（推荐）  <br>`对象名.变量名`（可用但不建议）                              | `类名::函数名()`（推荐）  <br>`对象名.函数名()`（合法但逻辑矛盾）<br>              |
| ​**this指针** | 无直接关联（属于类而非对象）                                                    | 禁止使用this指针（无法访问非静态成员）<br>                                  |
| ​**跨对象共享性** | 所有对象共享同一副本                                                        | 所有对象共享同一实现逻辑                                               |
| ​**典型应用场景** | 1. 类实例计数器  <br>2. 全局配置参数  <br>3. 资源池（如连接池）<br>                    | 1. 操作静态数据成员的接口  <br>2. 工具类函数（如数学计算）  <br>3. 工厂方法模式<br>     |