# Rust 泛型（Generics）核心知识点总结

---

## 一、泛型基础概念
• **核心作用**：编写可复用的代码，处理多种数据类型，避免重复逻辑（如为不同类型编写相同函数）
• **本质**：一种静态多态技术，通过类型参数化实现代码抽象
• **零成本抽象**：运行时无性能损耗（通过编译期单态化实现）

---

## 二、函数泛型
### 基本语法
```rust
fn largest<T: PartialOrd>(list: &[T]) -> T {
    let mut largest = list[0];
    // ...
}
```
### 关键点
1. **声明泛型参数**：`<T>` 在函数名后声明
2. **Trait 约束**：通过 `T: Trait` 指定类型能力（如 `PartialOrd` 实现比较）
3. **常见约束示例**：
   • `std::ops::Add`：支持 `+` 操作
   • `std::fmt::Display`：支持格式化输出
   • `Copy` / `Clone`：控制复制行为

---

## 三、结构体泛型
### 基本语法
```rust
struct Point<T> {
    x: T,
    y: T,
}
```
### 特性
1. **多泛型参数**：`struct Point<T, U> { x:T, y:U }`
2. **方法实现**：
   ```rust
   impl<T> Point<T> {
       fn x(&self) -> &T { &self.x }
   }
   ```
3. **具体类型特化**：
   ```rust
   impl Point<f32> {
       fn distance_from_origin(&self) -> f32 {
           (self.x.powi(2) + self.y.powi(2)).sqrt()
       }
   }
   ```

---

## 四、枚举泛型
### 典型应用
```rust
enum Option<T> {
    Some(T),
    None,
}

enum Result<T, E> {
    Ok(T),
    Err(E),
}
```
• `Option<T>`：处理值的存在性
• `Result<T, E>`：处理操作结果与错误

---

## 五、方法泛型
### 混合泛型参数
```rust
impl<T, U> Point<T, U> {
    fn mixup<V, W>(self, other: Point<V, W>) -> Point<T, W> {
        Point { x: self.x, y: other.y }
    }
}
```
• 结构体泛型与方法泛型相互独立
• 可组合不同类型实现灵活操作

---

## 六、const 泛型（值泛型）
### 处理固定大小数组
```rust
fn display_array<T: Debug, const N: usize>(arr: [T; N]) {
    println!("{:?}", arr);
}
```
### 特性
• 使用 `const N: usize` 声明值泛型
• 解决数组长度不同导致的类型问题
• 编译期确定值，支持更灵活的数据结构

---

## 七、单态化（Monomorphization）
### 工作原理
• 编译时为每个具体类型生成独立代码
• 示例：
  ```rust
  // 泛型代码
  let integer = Some(5);
  let float = Some(5.0);
  
  // 编译后生成
  enum Option_i32 { Some(i32), None }
  enum Option_f64 { Some(f64), None }
  ```
### 优劣分析
• **优势**：运行时效率等同手写具体类型代码
• **代价**：增加编译时间与生成文件大小

---

## 八、最佳实践
1. **合理使用 Trait 约束**：确保类型具备必要能力
2. **控制泛型参数数量**：避免过度复杂化（如 `Woo<T,U,V,W,X>`）
3. **优先选择 const 泛型**：处理数组长度等值相关场景
4. **平衡抽象与性能**：在代码复用和编译效率间取舍

---

## 附：常见错误解决方案
1. **缺少 Trait 约束**：
   ```rust
   // 错误：无法比较 T 类型
   fn largest<T>(list: &[T]) -> T {}
   
   // 修复：添加 PartialOrd 约束
   fn largest<T: PartialOrd>(list: &[T]) -> T {}
   ```
2. **类型推断失败**：
   ```rust
   create_and_print(); // 错误：无法推断 T 类型
   
   // 显式指定类型
   create_and_print::<i64>();
   ```

---

通过系统掌握泛型机制，可大幅提升 Rust 代码的抽象能力和复用性，同时保持高性能特性。建议结合实战项目深化理解，特别是在集合类型、算法实现等场景中实践应用。