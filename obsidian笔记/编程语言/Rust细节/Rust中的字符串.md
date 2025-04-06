# Rust 字符串系统全面解析

## 一、字符串类型体系
### 1.1 核心字符串类型
• **&str（字符串切片）**：
  • 不可变引用类型，可指向字符串字面量或String的切片
  • 编译时确定大小，存储在栈或程序的只读数据段
  • 类型表示为`&'static str`（字面量）或`&str`（动态切片）

• **String**：
  • 可变、堆分配的UTF-8字符串类型
  • 所有权类型，支持动态增长和修改
  • 底层存储为`Vec<u8>`的包装类型

### 1.2 其他字符串类型
• **OsString/OsStr**：处理操作系统原生格式的字符串
• **CString/CStr**：用于C语言交互的null-terminated字符串
• **PathBuf/Path**：专用于文件路径处理的类型

## 二、字符串切片机制
### 2.1 切片基础
• 语法：`[start..end]`（前闭后开区间）
• 内存结构：存储起始指针和长度（字节长度）
• 有效索引必须落在UTF-8字符边界
```rust
let s = String::from("中国人");
let valid = &s[0..3]; // "中"
// let invalid = &s[0..2]; 运行时panic
```

### 2.2 切片特性
• 零拷贝：不分配新内存
• 生命周期：依赖原字符串数据
• 通用性：同样适用于数组等连续集合
```rust
let arr = [1,2,3,4,5];
let slice = &arr[1..3]; // &[i32]类型
```

## 三、字符串操作详解
### 3.1 创建与转换
| 方法                  | 说明          |
| ------------------- | ----------- |
| String::from("str") | 字面量转String  |
| "str".to_string()   | 同前，语法糖形式    |
| s.as_str()          | String转&str |
| s[..]               | 显式切片转换      |
| &str.to_string()    | 字面量转string  |

### 3.2 修改操作
• **追加**：
  ```rust
  let mut s = String::new();
  s.push('€');      // 追加字符
  s.push_str("20");// 追加字符串
  ```

• **插入**：
  ```rust
  s.insert(5, '!');     // 插入字符
  s.insert_str(0, "Hi ");// 插入字符串
  ```

• **替换**：
  ```rust
  let s1 = s.replace("Hi", "Hello"); // 全量替换
  s.replacen("l", "L", 1);          // 替换指定次数
  s.replace_range(0..2, "HE");      // 范围替换
  ```

### 3.3 删除操作
| 方法         | 行为                         | 示例                     |
|-------------|-----------------------------|------------------------|
| pop()       | 移除最后一个字符返回Option   | s.pop().unwrap()      |
| remove(n)   | 移除并返回第n字节的字符       | s.remove(0)           |
| truncate(n) | 保留前n字节                  | s.truncate(3)         |
| clear()     | 等价truncate(0)              | s.clear()             |

### 3.4 字符串连接
• **+运算符**：
  ```rust
  let s1 = String::from("Hello");
  let s2 = String::from("Rust");
  let s3 = s1 + &s2;  // s1所有权转移
  ```

• **format!宏**：
  ```rust
  let s = format!("{}-{}", 2024, "year");
  ```

```rust
    string_slice("blue");
    string("red".to_string());
    string(String::from("hi"));
    string("rust is fun!".to_owned());
    string_slice("nice weather".into());
    string(format!("Interpolation {}", "Station"));
    string_slice(&String::from("abc")[0..1]);
    string_slice("  hello there ".trim());
    string("Happy Monday!".to_string().replace("Mon", "Tues"));
    string("mY sHiFt KeY iS sTiCkY".to_lowercase());
```
## 四、UTF-8处理机制
### 4.1 编码特性
• 动态字节长度：1-4字节/字符
• 禁止直接索引访问：
  ```rust
  let s = "नमस्ते";
  // let c = s[0]; 编译错误
  ```

### 4.2 遍历方法
• **字符级遍历**：
  ```rust
  for c in "中国人".chars() { /* char类型处理 */ }
  ```
  
• **字节级遍历**：
  ```rust
  for b in "中国人".bytes() { /* u8类型处理 */ }
  ```

### 4.3 子串处理
• 推荐使用`utf8_slice`库：
  ```rust
  utf8_slice::slice(s, 1, 3); // 安全字符切片
  ```

## 五、内存管理机制
### 5.1 所有权模型
• **String**：拥有堆内存的所有权
• **&str**：借用数据，无所有权

### 5.2 RAII机制
• 自动内存回收：
  ```rust
  {
      let s = String::from("temp"); // 分配内存
  } // 自动调用drop释放内存
  ```

## 六、典型错误场景
### 6.1 类型不匹配
```rust
fn greet(name: String) {}
let s = "Rust";
// greet(s); 错误！需s.to_string()
```

### 6.2 无效切片
```rust
let s = "αβγ";
// let sub = &s[0..1]; 运行时panic
```

### 6.3 借用冲突
```rust
let mut s = String::from("hello");
let slice = &s[..];
// s.clear(); 编译错误：存在不可变借用
```

## 七、最佳实践
1. 优先使用&str类型作为函数参数
2. 需要修改时选择String类型
3. 避免直接字节索引操作
4. 处理多字节字符时使用chars()遍历
5. 复杂字符串操作使用专用库（如regex、utf8_slice）

通过深入理解Rust的字符串系统，开发者可以避免常见陷阱，编写出安全高效的字符串处理代码。字符串设计的核心差异源于Rust的内存安全理念，在保证安全性的同时兼顾了灵活性。