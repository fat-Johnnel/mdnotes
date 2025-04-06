# Rust 模式匹配语法全解

## 一、基础模式匹配
### 1. 字面值匹配
• 直接匹配具体数值
```rust
match x {
    1 => println!("one"),
    2 => println!("two"),
    _ => println!("other")
}
```

### 2. 变量遮蔽
• 匹配分支内变量会覆盖外部同名变量
```rust
let x = Some(5);
let y = 10;
match x {
    Some(y) => println!("内部y: {}", y),  // 这里y=5
    _ => ()
}
```

## 二、组合匹配模式
### 1. 多模式匹配（|）
• 使用管道符匹配多个可能值
```rust
match x {
    1 | 2 => println!("1或2"),
    3 => println!("3"),
    _ => println!("其他")
}
```

### 2. 范围匹配（..=）
• 仅支持数值和字符类型
```rust
match x {
    1..=5 => println!("1-5"),
    'a'..='j' => println!("a-j")
}
```

## 三、解构模式
### 1. 结构体解构
```rust
struct Point { x: i32, y: i32 }
let p = Point { x: 0, y: 7 };

match p {
    Point { x: 0, y } => println!("Y轴"),
    Point { x, y: 0 } => println!("X轴"),
    Point { x, y } => println!("({}, {})", x, y)
}
```

### 2. 枚举解构
```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    ChangeColor(i32, i32, i32)
}

match msg {
    Message::Quit => println!("退出"),
    Message::Move { x, y } => println!("移动到({}, {})", x, y),
    Message::ChangeColor(r, g, b) => println!("颜色RGB({}, {}, {})", r, g, b)
}
```

### 3. 元组/数组解构
```rust
// 元组解构
let (a, b) = (1, 2);

// 数组解构
let arr = [1, 2, 3];
match arr {
    [first, .., last] => println!("首尾: {}, {}", first, last)
}
```

## 四、忽略模式
### 1. 单下划线 _
• 忽略单个值
```rust
fn foo(_: i32, y: i32) { /* 忽略第一个参数 */ }
```

### 2. 命名下划线变量
• 以下划线开头的变量不会触发未使用警告
```rust
let _unused = 5;  // 不会警告
```

### 3. 点运算符（..）
• 忽略剩余部分
```rust
struct Point3D { x: i32, y: i32, z: i32 }
match origin {
    Point3D { x, .. } => println!("x is {}", x)
}
```

## 五、高级模式
### 1. 匹配守卫（if）
• 在模式后添加条件判断
```rust
match num {
    Some(n) if n % 2 == 0 => println!("偶数"),
    Some(n) => println!("奇数"),
    None => ()
}
```

### 2. @绑定
• 绑定值到变量同时进行模式匹配
```rust
match msg {
    Message::Hello { id: id_variable @ 3..=7 } => {
        println!("ID在范围: {}", id_variable)
    }
}
```
也可以在绑定的同时对目标进行解构
```rust
#[derive(Debug)]
struct Point {
    x: i32,
    y: i32,
}

fn main() {
    // 绑定新变量 `p`，同时对 `Point` 进行解构
    let p @ Point {x: px, y: py } = Point {x: 10, y: 23};
    println!("x: {}, y: {}", px, py);
    println!("{:?}", p);


    let point = Point {x: 10, y: 5};
    if let p @ Point {x: 10, y} = point {
        println!("x is 10 and y is {} in {:?}", y, p);
    } else {
        println!("x was not 10 :(");
    }
}
```

## 六、注意事项
1. **范围匹配限制**：只能用于数值和字符类型
2. **模式一致性**：解构时必须类型完全匹配
3. **点运算符限制**：不能产生歧义（如`..`只能出现一次）
4. **变量遮蔽优先级**：匹配分支内的变量优先于外部作用域
5. **匹配守卫优先级**：`(4 | 5 | 6) if y` 等价于 `(4 | 5 | 6)`整体应用条件

## 七、典型应用场景
• 处理枚举的不同变体
• 解析复杂数据结构
• 输入参数验证
• 状态机转换处理
• 错误处理（结合Result类型）

> 通过组合这些模式语法，可以构建出精确而富有表达力的匹配逻辑。建议在实践中结合具体业务场景灵活运用，遇到特殊需求时再查阅具体语法细节。



## 附：全模式速查表
| 模式类型            | 语法示例                     | 适用场景                     |
|---------------------|-----------------------------|-----------------------------|
| 字面值匹配          | `1`, `"hello"`             | 精确值匹配                  |
| 变量绑定            | `Some(x)`                  | 值捕获                      |
| 或模式              | `1 | 2 | 3`              | 多值匹配                    |
| 范围模式            | `1..=5`, `'a'..='z'`      | 数值/字符范围               |
| 结构体解构          | `Point { x, y }`           | 字段提取                    |
| 元组解构            | `(a, b, c)`                | 位置匹配                    |
| 切片模式            | `[first, .., last]`        | 数组/切片处理               |
| 引用模式            | `&val`, `ref val`          | 引用处理                    |
| 守卫条件            | `x if x > 10`              | 附加条件检查                |
| 通配符              | `_`                        | 忽略值                      |
| 剩余模式            | `..`                       | 忽略多个值                  |
| @绑定模式           | `n @ 1..=10`               | 绑定+范围检查               |
| 嵌套模式            | `Some(Point { x, y })`     | 复杂数据结构处理            |
| 常量模式            | `const PI: f32 = 3.14;`    | 匹配预定义常量              |
