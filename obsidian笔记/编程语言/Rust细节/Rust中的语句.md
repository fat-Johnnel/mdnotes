## if表达式
常规表达式：
```rust
if number < 5 {
	statement;
} else if condition {
	statement;
} else {
	statement;
}
```

还可以在let语句右侧使用：
```rust
let number = if condition {5} else {0};
```

## 循环
循环可以使用``break``来退出，也可以使用``continue``来跳过本次循环

### 从循环返回值

```rust
fn main() {
    let mut counter = 0;
    let result = loop {
        counter += 1;
        if counter == 10 {
            break counter * 2;
        }
    };
    println!("The result is {}", result);
}

```

### 循环标签：


如果存在嵌套循环，break 和 continue 应用于此时最内层的循环。你可以选择在一个循环上指定一个 循环标签（loop label），然后将标签与 break 或 continue 一起使用，使这些关键字应用于已标记的循环而不是最内层的循环。

```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {}", count);
        let mut remaining = 10;
        loop {
            println!("remaining = {}", remaining);
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }
        count += 1;
    }
    println!("End count = {}", count);
}
```


### loop循环
``loop``循环会一直执行，直到用户明令退出
```rust
loop {
 statement;
}
```

### for循环
```rust
for x in 0..100{//也可以使用其他容器，0..100指的是[0,100)
	println!("{}",x);
}
```
一般地，使用其他容器时，选择引用，因为会发生所有权转移

| 使用方法                          | 等价使用方式                                            | 所有权   |
| ----------------------------- | ------------------------------------------------- | ----- |
| `for item in collection`      | `for item in IntoIterator::into_iter(collection)` | 转移所有权 |
| `for item in &collection`     | `for item in collection.iter()`                   | 不可变借用 |
| `for item in &mut collection` | `for item in collection.iter_mut()`               | 可变借用  |
|                               |                                                   |       |
如果想在循环中**获取元素的索引**：
```rust
fn main() {
    let a = [4, 3, 2, 1];
    // `.iter()` 方法把 `a` 数组变成一个迭代器
    for (i, v) in a.iter().enumerate() {
        println!("第{}个元素是{}", i + 1, v);
    }
}
```
### while循环
```rust
while condition{
	statement;
}
```


## 所有权概念

所有权规则
- Rust 中的每一个值都有一个所有者（owner）
- 值在任一时刻有且只有一个所有者
- 当所有者（变量）离开作用域，这个值将被丢弃

例如

```rust
let x = 123;
let y = x;
```
此时，123 这个值的所有者就是 y,而 x 则被废弃掉，不再使用,因为一个值在任何时候有且只有一个所有者。

### 函数参数的所有权
**直接传递参数，参数的所有权会转移**
例子：
```rust
fn main() {
    let s = String::from("hello");  // 创建字符串
    takes_ownership(s);            // s 的所有权被转移
    // s 在这里不再可用
    
    let x = 5;                     // 整数类型实现 Copy
    makes_copy(x);                 // x 的值被复制
    // x 在这里仍然可用
} // 这里 x 先移出作用域，然后是 s（但 s 的值已被移走）

fn takes_ownership(some_string: String) { // some_string 进入作用域
    println!("{}", some_string);
} // 这里 some_string 移出作用域并调用 drop 方法释放内存

fn makes_copy(some_integer: i32) { // some_integer 进入作用域
    println!("{}", some_integer);
} // 这里 some_integer 移出作用域（无特殊操作）
```