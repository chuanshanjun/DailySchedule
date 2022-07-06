# DaliySchedule
参加Open-Source OS Training Comp 2022的日常学习记录，虽然工作很忙很累但请坚持一下，谢谢 ^_^ ~

## Day 01 
* 完成 learn_rust_rustlings 环境配置
* 阅读完 《Rust圣经》 第一章 

## 1 cargo基本命令

### 1.1 cargo run & build

* cargo run 由以下两个命令组成(开发环境下使用)
    * 编译：cargo build
    * 运行：./target/debug/${project_name}，运行在debug模式下，**代码编译非常快，运行慢**
* cargo run --release (生产环境使用)
* cargo build --release
### 1.2 cargo check
* cargo check 快速检查代码是否可通过编译

## 2 Cargo.toml & Cargo.lock
* cargo.toml 项目数据描述文件
* cargo.lock 项目依赖清单   
```
项目是可运行程序时，上传Cargo.lock，如果是依赖则添加到.gitignore
```

* 项目依赖
```
[dependencies]
rand = "0.3" // 基于Rust官方仓库
hammer = { version = "0.5.0"}  // 同上
color = { git = "https://github.com/bjz/color-rs" } // 来源于git
geometry = { path = "crates/geometry" } // 来源于本地
```

* rust tips:
    * `println!` 后面带 `!` 代表是宏，不是函数
    * ` 在其语言中的占位符，可省略`
        ```Rust
        println!("{}, {}cm", name, length);
        ```