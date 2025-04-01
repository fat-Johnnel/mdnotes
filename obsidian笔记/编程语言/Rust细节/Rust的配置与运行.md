## cargo的使用
==创建新包==
``cargo new packagename``
==项目的结构==
```
$ tree
.
├── .git
├── .gitignore
├── Cargo.toml
└── src
    └── main.rs
```
==编译并运行(debug模式)==
``cargo run``
等价于``cargo build && ./target/debug/package``
==编译并运行(普通模式)==
``cargo run --release``
抑或``cargo build --release``
==检查语法是否正确（无需编译）==
``cargo check``

## cargo.toml和cargo.lock
`Cargo.toml` 和 `Cargo.lock` 是 `cargo` 的核心文件，它的所有活动均基于此二者。

- `Cargo.toml` 是 `cargo` 特有的**项目数据描述文件**。它存储了项目的所有元配置信息，如果 Rust 开发者希望 Rust 项目能够按照期望的方式进行构建、测试和运行，那么，必须按照合理的方式构建 `Cargo.toml`。
    
- `Cargo.lock` 文件是 `cargo` 工具根据同一项目的 `toml` 文件生成的**项目依赖详细清单**，因此我们一般不用修改它，只需要对着 `Cargo.toml` 文件撸就行了。



## Rust的编程元素（初步）
hello_world程序：
```rust
fn main(){
	println!("hello_world");
}
```