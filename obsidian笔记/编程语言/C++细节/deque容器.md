
| 类别        | 方法/操作                                                                            | 描述                                                                            | 时间复杂度 (平均)        |
| --------- | -------------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ----------------- |
| **构造与析构** | `deque()`                                                                        | 默认构造函数，创建一个空的 `deque`。                                                        | O(1)              |
|           | `deque(n)`                                                                       | 构造一个包含 `n` 个默认初始化元素的 `deque`。                                                 | O(n)              |
|           | `deque(n, val)`                                                                  | 构造一个包含 `n` 个值为 `val` 的元素的 `deque`。                                            | O(n)              |
|           | `deque(first, last)`                                                             | 构造一个包含迭代器 `[first, last)` 所指定范围内元素的 `deque`。                                  | O(n)              |
|           | `deque(const deque& other)`                                                      | 拷贝构造函数，创建一个与 `other` 相同的 `deque`。                                             | O(n)              |
|           | `deque(deque&& other)`                                                           | 移动构造函数，将 `other` 的资源转移到新的 `deque`，`other` 变为有效但不确定的状态。                        | O(1)              |
|           | `~deque()`                                                                       | 析构函数，销毁 `deque` 中的所有元素并释放内存。                                                  | O(n)              |
| **容量**    | `empty()`                                                                        | 检查 `deque` 是否为空，返回 `true` 如果为空，否则返回 `false`。                                  | O(1)              |
|           | `size()`                                                                         | 返回 `deque` 中元素的个数。                                                            | O(1)              |
|           | `max_size()`                                                                     | 返回 `deque` 可以容纳的最大元素个数，这通常取决于系统的可用内存。                                         | O(1)              |
|           | `resize(n)`                                                                      | 将 `deque` 的大小调整为 `n`。如果 `n` 小于当前大小，则删除尾部的元素；如果 `n` 大于当前大小，则在尾部添加默认初始化的元素。     | O(n)              |
|           | `resize(n, val)`                                                                 | 将 `deque` 的大小调整为 `n`。如果 `n` 小于当前大小，则删除尾部的元素；如果 `n` 大于当前大小，则在尾部添加值为 `val` 的元素。 | O(n)              |
| **元素访问**  | `operator[] (size_t n)`                                                          | 返回索引 `n` 处的元素的引用（不进行边界检查）。                                                    | O(1)              |
|           | `at(size_t n)`                                                                   | 返回索引 `n` 处的元素的引用（进行边界检查，如果索引越界会抛出 `std::out_of_range` 异常）。                    | O(1)              |
|           | `front()`                                                                        | 返回第一个元素的引用。如果 `deque` 为空，行为未定义。                                               | O(1)              |
|           | `back()`                                                                         | 返回最后一个元素的引用。如果 `deque` 为空，行为未定义。                                              | O(1)              |
| **修改器**   | `assign(n, val)`                                                                 | 将 `deque` 的内容替换为 `n` 个值为 `val` 的元素。                                           | O(n)              |
|           | `assign(first, last)`                                                            | 将 `deque` 的内容替换为迭代器 `[first, last)` 所指定范围内的元素。                                | O(n)              |
|           | `push_back(const T& val)`                                                        | 在 `deque` 的尾部添加一个元素 `val` (拷贝)。                                               | O(1) (摊销)         |
|           | `push_back(T&& val)`                                                             | 在 `deque` 的尾部添加一个元素 `val` (移动)。                                               | O(1) (摊销)         |
|           | `push_front(const T& val)`                                                       | 在 `deque` 的头部添加一个元素 `val` (拷贝)。                                               | O(1) (摊销)         |
|           | `push_front(T&& val)`                                                            | 在 `deque` 的头部添加一个元素 `val` (移动)。                                               | O(1) (摊销)         |
|           | `pop_back()`                                                                     | 移除 `deque` 尾部的元素。如果 `deque` 为空，行为未定义。                                         | O(1)              |
|           | `pop_front()`                                                                    | 移除 `deque` 头部的元素。如果 `deque` 为空，行为未定义。                                         | O(1)              |
|           | `insert(iterator pos, const T& val)`                                             | 在迭代器 `pos` 指向的位置之前插入一个元素 `val` (拷贝)，返回指向新插入元素的迭代器。                            | O(n)              |
|           | `insert(iterator pos, T&& val)`                                                  | 在迭代器 `pos` 指向的位置之前插入一个元素 `val` (移动)，返回指向新插入元素的迭代器。                            | O(n)              |
|           | `insert(iterator pos, size_t n, const T& val)`                                   | 在迭代器 `pos` 指向的位置之前插入 `n` 个值为 `val` 的元素。                                       | O(n)              |
|           | `insert(iterator pos, InputIterator first, InputIterator last)`                  | 在迭代器 `pos` 指向的位置之前插入迭代器 `[first, last)` 所指定范围内的元素。                            | O(m) (m 是插入元素的个数) |
|           | `erase(iterator pos)`                                                            | 移除迭代器 `pos` 指向的元素，返回指向被移除元素之后元素的迭代器。                                          | O(n)              |
|           | `erase(iterator first, iterator last)`                                           | 移除迭代器 `[first, last)` 所指定范围内的元素，返回指向被移除元素之后元素的迭代器。                            | O(n)              |
|           | `clear()`                                                                        | 移除 `deque` 中的所有元素，使其大小变为 0。                                                   | O(n)              |
|           | `swap(deque& other)`                                                             | 与另一个 `deque` 对象 `other` 交换内容。                                                 | O(1)              |
| **迭代器**   | `begin()`                                                                        | 返回指向 `deque` 第一个元素的迭代器。                                                       | O(1)              |
|           | `end()`                                                                          | 返回指向 `deque` 最后一个元素之后位置的迭代器。                                                  | O(1)              |
|           | `rbegin()`                                                                       | 返回指向 `deque` 最后一个元素的逆向迭代器。                                                    | O(1)              |
|           | `rend()`                                                                         | 返回指向 `deque` 第一个元素之前位置的逆向迭代器。                                                 | O(1)              |
|           | `cbegin()`                                                                       | 返回指向 `deque` 第一个元素的常量迭代器。                                                     | O(1)              |
|           | `cend()`                                                                         | 返回指向 `deque` 最后一个元素之后位置的常量迭代器。                                                | O(1)              |
|           | `crbegin()`                                                                      | 返回指向 `deque` 最后一个元素的常量逆向迭代器。                                                  | O(1)              |
|           | `crend()`                                                                        | 返回指向 `deque` 第一个元素之前位置的常量逆向迭代器。                                               | O(1)              |
| **比较操作符** | `operator==`, `operator!=`, `operator<`, `operator>`, `operator<=`, `operator>=` | 对两个 `deque` 对象进行比较操作，基于字典序比较元素。                                               | O(n)              |

**注意事项:**

* `deque` (double-ended queue) 是一种双端队列，允许在头部和尾部进行高效的插入和删除操作（平均时间复杂度为 O(1)）。
* 在 `deque` 的中间进行插入和删除操作的时间复杂度为 O(n)，因为需要移动其他元素。
* 与 `vector` 相比，`deque` 在头部插入和删除元素时不需要移动其他元素，因此效率更高。
* 与 `list` 相比，`deque` 支持随机访问（通过索引 `operator[]` 或 `at()`），而 `list` 不支持。
* `deque` 内部通常采用分段连续的存储方式，这可能导致迭代器失效的情况比 `vector` 更复杂，但只要不进行插入或删除操作，迭代器通常是有效的。
* 时间复杂度中的 "摊销" (amortized) 意味着虽然单个操作在最坏情况下可能需要 O(n) 的时间（例如，当内部需要重新分配内存时），但一系列连续操作的平均时间复杂度为 O(1)。

希望这个表格能够帮助你理解 C++ 中 `deque` 容器的用法。