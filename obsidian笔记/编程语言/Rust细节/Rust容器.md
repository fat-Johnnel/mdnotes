在 Rust 中，`Vec` 是动态数组，用于存储同类型元素的集合，能够自动调整大小。以下是 `Vec` 的详细用法，包含常见操作和代码示例：

---

### 一、创建 `Vec`
#### 1. 创建空 `Vec`
```rust
let mut vec: Vec<i32> = Vec::new(); // 空 Vec，需指定类型
let mut vec = Vec::<i32>::new();    // 另一种写法
```

#### 2. 使用宏 `vec!` 初始化
```rust
let vec = vec![1, 2, 3]; // 类型自动推断为 Vec<i32>
let names = vec!["Alice", "Bob"];
```

#### 3. 指定容量（优化性能）
```rust
let mut vec = Vec::with_capacity(10); // 预分配容量为 10
```

---

### 二、添加元素
#### 1. 末尾添加元素：`push`
```rust
let mut vec = vec![1, 2];
vec.push(3); // vec: [1, 2, 3]
```

#### 2. 插入元素到指定位置：`insert`
```rust
vec.insert(1, 5); // 在索引 1 处插入 5 → [1, 5, 2, 3]
```

---

### 三、访问元素
#### 1. 通过索引：直接访问（可能 panic）
```rust
let first = vec[0]; // 1，索引越界会导致 panic
```

#### 2. 安全访问：`get` 返回 `Option<T>`
```rust
match vec.get(2) {
    Some(x) => println!("元素是 {}", x),
    None => println!("索引越界"),
}
```

#### 3. 访问最后一个元素：`.last()`
```rust
let last = vec.last(); // Some(&3)
```

---

### 四、删除元素
#### 1. 移除末尾元素：`pop`
```rust
let last = vec.pop(); // 返回 Option<T>，例如 Some(3)
```

#### 2. 移除指定位置元素：`remove`
```rust
let removed = vec.remove(1); // 移除索引 1 处的元素 → 5
```

#### 3. 清空 `Vec`：`clear`
```rust
vec.clear(); // 清空所有元素，容量不变
```

---

### 五、遍历元素
#### 1. 不可变遍历
```rust
for num in &vec {
    println!("{}", num); // 通过引用遍历，不转移所有权
}
```

#### 2. 可变遍历
```rust
for num in &mut vec {
    *num += 1; // 修改元素值
}
```

#### 3. 转移所有权的遍历
```rust
for num in vec {
    println!("{}", num); // 转移所有权，遍历后 vec 不可用
}
```

---

### 六、常用方法
#### 1. 获取长度和容量
```rust
let len = vec.len();      // 当前元素个数
let cap = vec.capacity(); // 预分配的内存容量
```

#### 2. 判断是否为空
```rust
if vec.is_empty() {
    println!("Vec 是空的");
}
```

#### 3. 保留指定容量：`reserve`
```rust
vec.reserve(20); // 确保容量至少为 20
```

#### 4. 缩减容量：`shrink_to_fit`
```rust
vec.shrink_to_fit(); // 容量缩减至与长度一致
```

---

### 七、切片（Slice）
#### 1. 转换为不可变切片
```rust
let slice: &[i32] = &vec[..]; // 获取全部元素的切片
let partial = &vec[1..3];      // 获取索引 1 到 2 的切片
```

#### 2. 转换为可变切片
```rust
let slice_mut: &mut [i32] = &mut vec[..];
slice_mut[0] = 100; // 修改原 Vec 的值
```

---

### 八、转换操作
#### 1. 从数组创建 `Vec`
```rust
let arr = [1, 2, 3];
let vec = arr.to_vec(); // Vec<i32>
```

#### 2. 从迭代器创建 `Vec`
```rust
let vec: Vec<_> = (1..5).collect(); // [1, 2, 3, 4]
```

#### 3. 合并两个 `Vec`：`append`
```rust
let mut vec1 = vec![1, 2];
let mut vec2 = vec![3, 4];
vec1.append(&mut vec2); // vec1: [1, 2, 3, 4], vec2 为空
```

---

### 九、内存与所有权
• **所有权规则**：`Vec` 拥有其元素，当 `Vec` 离开作用域时，所有元素会被自动释放。
• **元素所有权**：如果元素是 `String` 或另一个 `Vec`，它们的资源也会被释放。

---

### 十、示例代码
```rust
fn main() {
    // 1. 创建 Vec
    let mut numbers = vec![10, 20, 30];
    
    // 2. 添加元素
    numbers.push(40);
    numbers.insert(1, 15); // [10, 15, 20, 30, 40]
    
    // 3. 访问元素
    println!("第三个元素: {}", numbers[2]); // 20
    
    // 4. 删除元素
    numbers.remove(3); // 移除 30 → [10, 15, 20, 40]
    
    // 5. 遍历修改
    for num in &mut numbers {
        *num *= 2;
    } // numbers: [20, 30, 40, 80]
    
    // 6. 转换为切片
    let slice = &numbers[1..3]; // [30, 40]
    
    // 7. 合并 Vec
    let mut others = vec![100, 200];
    numbers.append(&mut others); // numbers: [20, 30, 40, 80, 100, 200]
}
```

---

### 十一、常见问题
1. **索引越界**：直接通过索引访问（如 `vec[100]`）会导致 panic，建议用 `get` 方法。
2. **迭代中修改**：避免在 `for in` 循环中直接修改 `Vec`，可能导致未定义行为。
3. **容量与性能**：频繁插入元素时，预分配容量（`with_capacity`）可减少内存重分配次数。

通过掌握这些操作，你可以高效地使用 `Vec` 处理动态集合数据。