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
        
## Day 02 
* 阅读完 《Rust圣经》 2.1 并完成练习
* 完成 learn_rust_rustlings intro 与 variables 的练习

## Day 03
* 阅读完 《Rust圣经》 2.2 并完成练习
* 完成 learn_rust_rustlings function,if,quiz1的练习

### 2.2 基本类型
### 2.2.1 数值类型

* Rust整形默认使用`i32`

**整型溢出**
* debug模式编译时，Rust会检查整型溢出，若存在则崩溃
* `--release`参数进行构建时，Rust **不**检测溢出。如有Rust会按照补码循环溢出

**浮点类型**
* `f32`和`f64`，默认浮点类型是`f64`，在现代CPU中它的速度与`f32`几乎相同，但精度更好

需要遵守以下准则:  
* 避免在浮点数上测试相等性
* 当结果在数学上可能存在未定义时，需要格外小心

```Rust
// 因为二进制精度的问题，导致0.1+0.2并不严格等于0.3
// 它们可能在小数点N位后存在误差。
// 非要比较可以采用 (0.1_f64 + 0.2 - 0.3).abs() < 0.00001
fn main() {
  // 断言0.1 + 0.2与0.3相等
  assert!(0.1 + 0.2 == 0.3);
}
```

**字符类型**
所有`Unicode`值都可以作为Rust字符，由于`Unicode`都是4个字节编码，因此字符类型也是占用4个字节。字符只能使用 **``** 来表示，**""** 留给字符串。

**布尔**
两种:`true` 和 `false`,布尔值占用内存1个字节

**单元类型**
单元类型就是`()`,例如 `fn main()` 函数返回单元类型`()`, 也可使用`()`作为`map`的值,表示我们不关注具体的值,只关注`key`。这种用法和Go语言的struct{}类似,可以作为一个值用来站位,但是完全**不占用**任何内存

**语句和表达式**
```Rust
fn add_with_extra(x: i32, y: i32) -> i32 {
    let x = x + 1; // 语句
    let y = y + 5; // 语句
    x + y // 表达式
}
```
语句会执行一些操作但是不会返回一个值,而表达式会在求值后返回一个值，因此在上述函数体的三行代码中，前两行是语句，最后一行是表达式。
对于Rust语言而言,**这种基于语句和表达式的方式是非常重要的，你需要能明确的区分这两个概念**,但是对于很多其它语言而言，这两个往往无需区分。基于表达式是函数式语言的重要特征,**表达式总要返回值**

由于`let`是语句，因此不能将`let`语句赋值给其它值，如下形式是错误的:
```Rust
let b = (let a = 8);
```

**表达式**
表达式可以成为语句的一部分,例如`let y = 6`中, `6` 就是一个表达式,它在求值后返回一个值 `6`  
调用一个函数是表达式,因为会返回一个值,调用宏也是表达式,用花括号包裹最终返回一个值的语句块也是表达式,总之,能返回值,它就是表达式
```Rust
fn main() {
    let y = {
        let x = 3;
        x + 1
    };

    println!("The value of y is: {}", y);
}
``` 
```Rust
{
    let x = 3;
    x + 1
}
```
该语句块是表达式的原因是:它的最后一行是表达式,返回了`x + 1`的值,注意 `x + 1` 不能以分号结尾,否则就会从表达式变成语句,**表达式不能包含分号**,这一点非常重要,一旦你在表达式后加上分号,它就会变成一条语句,再也**不会**返回一个值,请牢记！

最后,表达式如果不返回任何值,会隐式地返回一个`()`
```Rust
// assert_eq! 宏后面可加可不加 分号
fn main() {
    assert_eq!(ret_unit_type(), ())
}

fn ret_unit_type() {
    let x = 1;
    // if 语句块也是一个表达式，因此可以用于赋值，也可以直接返回
    if (x > 1) {

    }
}
```

**函数**

**无返回值**
单元类型`()`,是一个零长度地元组，它没啥用，但是可以用来表达一个函数没有返回值:  
* 函数没有返回值,那么返回一个`()`
* 通过`;`结尾地表达式返回一个`()`
```Rust
//例如下面的 report 函数会隐式返回一个 ()：
fn report<T: Debug>(item: T) {
  println!("{:?}", item);

}
// 与上面的函数返回值相同，但是下面的函数显式的返回了 ()：
fn clear(text: &mut String) -> () {
  *text = String::from("");
}
// !注意表达式加;号 返回的是()
fn add(x:u32,y:u32) -> u32 {
    x + y;
}
```

