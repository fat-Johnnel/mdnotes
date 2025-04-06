# Rust 模式匹配知识点总结

## 一、基本概念
1. **模式定义**  
   • 用于匹配数据结构中的值和结构
   • 由以下元素组合构成：
     ```text
     字面值 | 解构结构 | 变量 | 通配符(_) | 占位符(...)
     ```

2. **核心语法结构**  
   • `match` 表达式（穷尽匹配）：
     ```rust
     match VALUE {
         PATTERN => EXPRESSION,
         _ => EXPRESSION // 处理剩余情况
     }
     ```

## 二、主要应用场景

### 1. `match` 表达式
• **穷尽性检查**：必须覆盖所有可能情况
• 示例：
  ```rust
  match some_value {
      1 => println!("One"),
      2..=5 => println!("Two to Five"),
      _ => println!("Other")
  }
  ```

### 2. `if let` 条件匹配
• **单分支匹配**：处理特定模式，忽略其他情况
• 示例：
  ```rust
  if let Some(x) = some_option {
      println!("Got value: {}", x);
  }
  ```

### 3. `while let` 循环匹配
• **持续匹配**：只要模式匹配就执行循环
• 示例（栈操作）：
  ```rust
  let mut stack = vec![1, 2, 3];
  while let Some(top) = stack.pop() {
      println!("{}", top);
  }
  ```

### 4. `for` 循环解构
• **迭代器解构**：配合 `enumerate` 使用
• 示例：
  ```rust
  let v = vec!['a', 'b', 'c'];
  for (index, value) in v.iter().enumerate() {
      println!("{} at index {}", value, index);
  }
  ```

### 5. `let` 绑定
• **变量模式匹配**：基础变量绑定也是模式匹配
• 示例：
  ```rust
  let (x, y, z) = (1, 2, 3); // 元组解构
  let Point { x, y } = point; // 结构体解构
  ```

### 6. 函数参数匹配
• **参数解构**：直接解构传入参数
• 示例：
  ```rust
  fn print_coords(&(x, y): &(i32, i32)) {
      println!("({}, {})", x, y);
  }
  ```

## 三、常见模式类型

1. **变量绑定模式**
   ```rust
   let x = 5; // 最简单的变量绑定
   ```

2. **解构模式**
   • 支持结构体/元组/枚举解构
   ```rust
   struct Point { x: i32, y: i32 }
   let p = Point { x: 0, y: 7 };
   let Point { x, y } = p;
   ```

3. **通配符模式**
   ```rust
   let _ = 5; // 忽略值
   ```

4. **字面量模式**
   ```rust
   match 1 {
       1 => "one",
       2 => "two",
       _ => "other"
   }
   ```

## 四、注意事项

### 1. 类型匹配严格性
• 匹配双方类型必须完全一致
• 错误示例：
  ```rust
  let (x, y) = (1, 2, 3); // 元组元素数量不匹配
  ```

### 2. 穷尽性检查
• `match` 和 `let` 必须覆盖所有可能性
• 错误示例：
  ```rust
  let Some(x) = some_option; // 缺少 None 处理
  ```

### 3. 不可驳 vs 可驳模式
| 特征         | 不可驳模式               | 可驳模式                 |
|--------------|--------------------------|--------------------------|
| 使用场景     | `let`/函数参数           | `if let`/`while let`     |
| 匹配要求     | 必须成功                 | 允许失败                 |
| 典型示例     | `let (x, y) = (1, 2)`    | `if let Some(x) = opt`   |

### 4. 变量遮蔽问题
• 在模式匹配中可能意外创建新变量
• 示例：
  ```rust
  let x = 5;
  match x {
      x => println!("{}", x) // 这里创建了新变量x
  }
  ```

## 五、高级特性
1. **模式守卫（Pattern Guards）**
   ```rust
   match some_value {
       x if x < 5 => "less than 5",
       x => "5 or more"
   }
   ```

2. `@` 绑定
   ```rust
   match 1..=10 {
       x @ 1..=5 => println!("Small: {}", x),
       x => println!("Big: {}", x)
   }
   ```

3. **模式组合**
   ```rust
   match (x, y) {
       (0, 0) | (0, 1) => "origin edge",
       _ => "other"
   }
   ```