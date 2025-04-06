# Rust 结构体学习笔记

## 一、结构体基础
### 1. 结构体定义
• 由`struct`关键字定义，命名遵循大驼峰式
• 包含多个有名称的字段（与元组不同）
• 示例：
  ```rust
  struct User {
      active: bool,
      username: String,
      email: String,
      sign_in_count: u64,
  }
  ```

### 2. 实例化
• 必须初始化所有字段
• 字段顺序可自定义
• 示例：
  ```rust
  let user1 = User {
      email: String::from("user@example.com"),
      username: String::from("user123"),
      active: true,
      sign_in_count: 1,
  };
  ```

### 3. 字段访问与修改
• 使用点号`.`访问字段
• 实例必须声明为`mut`才能修改字段
• 示例：
  ```rust
  let mut user1 = ...;
  user1.email = String::from("new@example.com");
  ```

## 二、高级特性
### 1. 字段初始化简写
• 当参数名与字段名相同时可简写
• 示例：
  ```rust
  fn build_user(email: String, username: String) -> User {
      User {
          email,    // 等价于 email: email
          username, // 等价于 username: username
          active: true,
          sign_in_count: 1,
      }
  }
  ```

### 2. 结构体更新语法
• 使用`..`语法继承其他实例字段
• 必须放在初始化列表末尾
• 涉及所有权转移（未实现Copy的字段会转移）
• 示例：
  ```rust
  let user2 = User {
      email: String::from("new@example.com"),
      ..user1  // 继承其他字段
  };
  ```

## 三、特殊结构体类型
### 1. 元组结构体
• 无字段名，按位置访问
• 适用于需要类型区分但不需要字段名的场景
• 示例：
  ```rust
  struct Color(i32, i32, i32);
  struct Point(i32, i32, i32);
  let black = Color(0, 0, 0);
  ```

### 2. 单元结构体
• 无任何字段
• 用于需要实现trait但不需要存储数据的场景
• 示例：
  ```rust
  struct AlwaysEqual;
  impl SomeTrait for AlwaysEqual {}
  ```

## 四、内存布局
• 结构体字段连续排列（编译器可能优化）
• 包含所有权的字段（如String）存储指针
• 示例：
  ```rust
  struct File {
      name: String,    // 指向堆内存的指针
      data: Vec<u8>,   // 包含指针、容量、长度
  }
  ```

## 五、调试与输出
### 1. Debug特征
• 派生实现：`#[derive(Debug)]`
• 使用`{:?}`（简洁格式）或`{:#?}`（美化格式）
• 示例：
  ```rust
  #[derive(Debug)]
  struct Rectangle { width: u32, height: u32 }
  println!("{:?}", rect);  // 简洁输出
  println!("{:#?}", rect); // 格式化输出
  ```

### 2. dbg!宏
• 输出到标准错误流(stderr)
• 自动捕获表达式值和代码位置
• 示例：
  ```rust
  let width = dbg!(30 * scale); // 输出计算过程
  dbg!(&rect);                  // 输出结构体信息
  ```

## 六、注意事项
1. **所有权问题**：
   • String等未实现Copy的字段在结构体更新时会发生所有权转移
   • 转移部分字段后，原实例其他字段仍可访问

2. **引用字段**：
   ```rust
   struct User {
       username: &str,  // 错误！需要生命周期标注
   }
   ```
   • 需要生命周期参数：`struct User<'a> { username: &'a str }`

3. **可见性规则**：
   • 默认模块私有
   • 需用`pub`关键字公开结构体和字段

4. **模式匹配**：
   ```rust
   let User { username, .. } = user1;
   ```
   • 可用于解构结构体字段

## 七、最佳实践
1. 优先使用具名结构体而非元组结构体
2. 需要打印调试信息时自动派生Debug特征
3. 需要自定义显示格式时手动实现Display特征
4. 大结构体建议使用`Box`堆分配避免栈溢出
5. 包含引用的结构体要谨慎处理生命周期

## 八、综合示例
```rust
#[derive(Debug)]
struct Employee<'a> {
    name: String,
    department: &'a str,
    years: u8,
}

fn main() {
    let dept = "Engineering";
    let emp1 = Employee {
        name: String::from("Alice"),
        department: dept,
        years: 5,
    };
    
    let emp2 = Employee {
        name: String::from("Bob"),
        ..emp1  // 借用department字段
    };
    
    println!("{:#?}", emp2);
    dbg!(&emp2);
}
```