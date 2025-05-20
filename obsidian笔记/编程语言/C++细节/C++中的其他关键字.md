## 内存分配类
#### new
1. 基本用法：
```cpp
// 分配一个 int 类型的内存空间，并返回指向该空间的指针
int* ptrInt = new int;

// 分配一个 double 类型的内存空间，并返回指向该空间的指针
double* ptrDouble = new double;

// 分配一个 MyClass 类的对象，并返回指向该对象的指针（调用默认构造函数）
MyClass* obj1 = new MyClass;

// 分配一个 MyClass 类的对象，并返回指向该对象的指针（调用带参数的构造函数）
MyClass* obj2 = new MyClass(argument1, argument2);
```
2. 示例
```cpp
//假如定义了如下结构体：
struct ListNode {
    int val;
    ListNode *next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode *next) : val(x), next(next) {}
};

//则可以使用如下方式分配内存
// 创建一个值为 10 的新节点，其 next 指针为空
ListNode* head = new ListNode(10);

// 创建另一个值为 20 的新节点，其 next 指针为空
ListNode* second = new ListNode(20);

// 将第一个节点的 next 指针指向第二个节点，形成一个包含两个节点的链表
head->next = second;

// 创建第三个值为 30 的新节点，并将其连接到第二个节点
ListNode* third = new ListNode(30);
second->next = third;

// 现在，head 指向链表的第一个节点： 10 -> 20 -> 30 -> nullptr
```