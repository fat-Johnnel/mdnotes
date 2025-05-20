
## C++ 中的 unordered_map 和 unordered_set 用法

`unordered_map` 和 `unordered_set` 是 C++ 标准库 `<unordered_map>` 头文件中提供的关联容器，它们实现了**哈希表**（hash table）的数据结构。与 `std::map` 和 `std::set` 基于红黑树实现不同，`unordered_map` 和 `unordered_set` 提供了**平均常数时间复杂度 O(1)** 的元素查找、插入和删除操作。然而，在最坏情况下（例如，哈希冲突严重），这些操作的时间复杂度可能会退化到 O(n)，其中 n 是容器中元素的数量。

### 1. unordered_map

`unordered_map` 是一个存储 **键值对 (key-value pairs)** 的容器，其中每个键都是唯一的。它允许你根据键快速访问对应的值。

**头文件:**

```c++
#include <unordered_map>
````

**基本用法:**

  * **声明:**

    ```c++
    std::unordered_map<KeyType, ValueType> myMap;
    std::unordered_map<std::string, int> ageMap;
    std::unordered_map<int, std::string> nameMap;
    ```

  * **插入元素:**

    ```c++
    ageMap["Alice"] = 30; // 使用下标操作符
    ageMap.insert({"Bob", 25}); // 使用 insert 方法，参数是 pair
    ageMap.insert(std::make_pair("Charlie", 35)); // 同样使用 insert 方法
    ```

  * **访问元素:**

    ```c++
    int aliceAge = ageMap["Alice"]; // 使用下标操作符访问，如果键不存在会插入默认值
    int bobAge = ageMap.at("Bob"); // 使用 at 方法访问，如果键不存在会抛出 std::out_of_range 异常

    if (ageMap.count("David")) {
        std::cout << "David 的年龄是: " << ageMap["David"] << std::endl;
    } else {
        std::cout << "David 不在 ageMap 中。" << std::endl;
    }
    ```

  * **查找元素:**

    ```c++
    auto it = ageMap.find("Alice");
    if (it != ageMap.end()) {
        std::cout << "找到 Alice，年龄是: " << it->second << std::endl; // it 是迭代器，first 是键，second 是值
    } else {
        std::cout << "未找到 Alice。" << std::endl;
    }
    ```

  * **删除元素:**

    ```c++
    ageMap.erase("Bob"); // 通过键删除
    ageMap.erase(it); // 通过迭代器删除
    ageMap.erase(ageMap.begin(), ageMap.end()); // 删除所有元素
    ageMap.clear(); // 删除所有元素
    ```

  * **检查大小和是否为空:**

    ```c++
    size_t size = ageMap.size();
    bool isEmpty = ageMap.empty();
    ```

  * **迭代器:**

    ```c++
    for (const auto& pair : ageMap) {
        std::cout << pair.first << ": " << pair.second << std::endl;
    }

    for (auto it = ageMap.begin(); it != ageMap.end(); ++it) {
        std::cout << it->first << ": " << it->second << std::endl;
    }
    ```

### 2\. unordered\_set

`unordered_set` 是一个存储 **唯一元素** 的容器。它不允许存储重复的元素，并且提供了快速的元素查找、插入和删除操作。

**头文件:**

```c++
#include <unordered_set>
```

**基本用法:**

  * **声明:**

    ```c++
    std::unordered_set<ElementType> mySet;
    std::unordered_set<int> numbers;
    std::unordered_set<std::string> names;
    ```

  * **插入元素:**

    ```c++
    numbers.insert(10);
    numbers.insert(20);
    numbers.insert(10); // 重复元素不会被插入
    ```

  * **查找元素:**

    ```c++
    if (numbers.count(20)) {
        std::cout << "20 存在于 numbers 中。" << std::endl;
    } else {
        std::cout << "20 不存在于 numbers 中。" << std::endl;
    }

    auto it = numbers.find(15);
    if (it != numbers.end()) {
        std::cout << "找到 15。" << std::endl;
    } else {
        std::cout << "未找到 15。" << std::endl;
    }
    ```

  * **删除元素:**

    ```c++
    numbers.erase(10); // 通过值删除
    numbers.erase(it); // 通过迭代器删除
    numbers.erase(numbers.begin(), numbers.end()); // 删除所有元素
    numbers.clear(); // 删除所有元素
    ```

  * **检查大小和是否为空:**

    ```c++
    size_t size = numbers.size();
    bool isEmpty = numbers.empty();
    ```

  * **迭代器:**

    ```c++
    for (const auto& element : numbers) {
        std::cout << element << std::endl;
    }

    for (auto it = numbers.begin(); it != numbers.end(); ++it) {
        std::cout << *it << std::endl;
    }
    ```

### 3\. 自定义哈希函数和相等比较函数

对于自定义类型作为 `unordered_map` 的键或 `unordered_set` 的元素，你需要提供自定义的哈希函数和相等比较函数。这可以通过以下方式实现：

  * **函数对象 (Functor):** 创建一个实现了 `operator()` 的类。
  * **Lambda 表达式 (C++11 及更高版本):** 提供一个 lambda 函数。
  * **特化 `std::hash` 模板:** 为你的自定义类型特化 `std::hash` 模板。

**示例 (使用函数对象):**

```c++
struct Person {
    std::string name;
    int age;

    bool operator==(const Person& other) const {
        return name == other.name && age == other.age;
    }
};

struct PersonHash {
    size_t operator()(const Person& p) const {
        std::hash<std::string> nameHasher;
        std::hash<int> ageHasher;
        return nameHasher(p.name) ^ ageHasher(p.age); // 组合哈希值
    }
};

std::unordered_set<Person, PersonHash> peopleSet;
std::unordered_map<Person, std::string, PersonHash> personMap;

Person alice{"Alice", 30};
peopleSet.insert(alice);
personMap[alice] = "Engineer";
```

**总结:**

`unordered_map` 和 `unordered_set` 是在需要快速查找、插入和删除操作且不关心元素顺序时非常有用的容器。它们基于哈希表实现，提供了平均常数时间复杂度的操作，但在使用自定义类型作为键或元素时，需要提供自定义的哈希函数和相等比较函数。理解它们的用法和特性，可以帮助你更有效地解决各种编程问题。
