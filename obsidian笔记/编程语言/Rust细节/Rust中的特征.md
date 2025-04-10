---

### Rust 特征（Trait）笔记

---

#### 一、特征的基本概念
**特征（Trait）** 是 Rust 中定义共享行为的抽象机制，类似于其他语言的接口（Interface）。  
**核心作用**：  
• 将一组方法组合成行为集合，不同类型可实现同一特征，从而共享行为。
• 通过特征约束（Trait Bounds）实现多态和代码复用。

**示例**：定义一个“可总结内容”的特征：
```rust
pub trait Summary {
    fn summarize(&self) -> String;
}
```

---

#### 二、为类型实现特征
**语法**：`impl Trait for Type`  
**示例**：为 `Post` 和 `Weibo` 实现 `Summary` 特征：
```rust
struct Post { title: String, content: String }
impl Summary for Post {
    fn summarize(&self) -> String {
        format!("文章：{}", self.title)
    }
}

struct Weibo { username: String, content: String }
impl Summary for Weibo {
    fn summarize(&self) -> String {
        format!("@{}: {}", self.username, self.content)
    }
}
```

---

#### 三、默认方法实现
特征方法可提供默认实现，类型可选择使用或覆盖：
```rust
pub trait Summary {
    // 默认实现
    fn summarize(&self) -> String {
        String::from("（阅读更多...）")
    }
}

// 直接使用默认实现
struct News { source: String }
impl Summary for News {} 

// 覆盖默认实现
impl Summary for Weibo {
    fn summarize(&self) -> String {
        format!("@{}: {}", self.username, self.content)
    }
}
```

---

#### 四、特征作为函数参数与返回值
1. **参数语法糖** `impl Trait`：
```rust
fn notify(item: &impl Summary) {
    println!("摘要：{}", item.summarize());
}
```

2. **返回值** `impl Trait`（只能返回一种具体类型）：
```rust
fn get_weibo() -> impl Summary {
    Weibo {
        username: String::from("rustacean"),
        content: String::from("今天学 Rust 了！"),
    }
}
```

---

#### 五、特征约束（Trait Bounds）
1. **多特征约束**：
```rust
// 语法糖形式
fn notify(item: &(impl Summary + Display)) { /* ... */ }

// 完整形式
fn notify<T: Summary + Display>(item: &T) { /* ... */ }
```

2. **`where` 子句简化复杂约束**：
```rust
fn some_function<T, U>(t: &T, u: &U) -> i32
where
    T: Summary + Clone,
    U: Debug + PartialOrd,
{
    /* ... */
}
```

---

#### 六、孤儿规则（Orphan Rule）
• **规则**：为类型 `A` 实现特征 `T` 时，`A` 或 `T` 必须至少有一个在当前作用域定义。
• **示例**：
  • ✅ 允许：为自己定义的 `Post` 实现标准库的 `Display` 特征。
  • ❌ 禁止：为标准库的 `String` 实现自己定义的 `Summary` 特征（除非 `Summary` 是你自己定义的）。

---

#### 七、条件实现与自动派生
1. **条件实现**：
```rust
// 仅为实现了 Display 的类型实现 ToString 特征
impl<T: Display> ToString for T {
    /* ... */
}
```

2. **自动派生** `#[derive]`：
```rust
#[derive(Debug, Clone, PartialEq)]
struct Point {
    x: i32,
    y: i32,
}
```

---

#### 八、实际应用示例
1. **运算符重载**（实现 `Add` 特征）：
```rust
use std::ops::Add;

#[derive(Debug)]
struct Point { x: i32, y: i32 }

impl Add for Point {
    type Output = Self;
    fn add(self, other: Self) -> Self {
        Point {
            x: self.x + other.x,
            y: self.y + other.y,
        }
    }
}

fn main() {
    let p1 = Point { x: 1, y: 2 };
    let p2 = Point { x: 3, y: 4 };
    println!("{:?}", p1 + p2); // 输出：Point { x: 4, y: 6 }
}
```

2. **自定义格式化输出**（实现 `Display` 特征）：
```rust
use std::fmt;

struct File { name: String, size: u64 }

impl fmt::Display for File {
    fn fmt(&self, f: &mut fmt::Formatter) -> fmt::Result {
        write!(f, "文件：{}（大小：{}字节）", self.name, self.size)
    }
}

fn main() {
    let file = File { name: String::from("data.txt"), size: 1024 };
    println!("{}", file); // 输出：文件：data.txt（大小：1024字节）
}
```

---

### 总结
**特征（Trait）** 是 Rust 中实现多态和代码复用的核心工具：
1. **抽象共享行为**：定义“能做什么”的规范。
2. **灵活的类型交互**：通过特征约束，函数可接受或返回多种类型。
3. **代码复用与安全**：默认方法、条件实现和自动派生减少重复代码，同时通过孤儿规则保证安全性。

**核心价值**：让不同类型在统一的行为规范下协作，提升代码的抽象能力和可维护性。