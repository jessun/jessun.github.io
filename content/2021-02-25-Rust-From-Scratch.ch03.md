+++
title = "Rust From Scratch chap.03 | Rust 的类型系统"
draft = false

[taxonomies]
tags = ["rust"]
categories = ["Rust From Scratch"]
+++

---

# 1. 基本数据类型

## 1.1 整型

u8/i8/usize/isize 等

## 1.2 浮点型

f32/f64 等

## 1.3 字符串

### 1.3.1 str -> &str

`str`类型是一个动态大小的类型，表示字符串切片。`&str`引用被称为胖指针，表示栈上存储一个指向静态区域或者是堆内存的指针，以及数据的长度。
因为它比普通指针多占用一些空间（记录数据长度等元信息），所以叫做胖指针。
对于字面量来说，字符串可以存储在静态数据区。栈内存只存指针，所以它是一个静态引用字符串切片类型。

### 1.3.2 [T] -> &[T]

u8 序列切片和字符串切片的区别在于：字符串切片必然是 UTF-8 编码的合法字符串。u8 序列切片不一定是合法的 UTF-8 编码字符串。

### 1.3.3 String -> Vec<u8>

String 是存储在堆上的，具有合法 UTF-8 编码的 u8 动态数组序列。u8 动态数组序列不一定是合法的 UTF-8 编码的字符串。

### 1.3.4 其他（TODO）

## 1.4 指针类型

### 1.4.1 原始指针

`*mut T`和`*const T`，等价于 C 语言中的指针，可以为空，一般用于 unsafe Rust 中。

### 1.4.2 NonNull 指针

NonNull 指针是 Rust 语言建议的 *mut T 指针的替代指针。NonNull 指针是非空指针，并且是遵循生命周期类型协变规则。

### 1.4.3 函数指针

函数指针是指向代码的指针，而非数据。你可以使用它直接调用函数。

## 1.5 引用

 - 不可变引用/共享引用，`&T`
 - 可变引用/独占引用，`&mut T`

引用和指针的主要区别：

 - 引用不可能为空
 - 拥有生命周期
 - 受借用检查器保护不会发生悬垂指针等问题

## 1.6 元组

 - Rust 中唯一的异构序列，可以在元组中放入不同类型的值。
 - 不同长度的元组是不同类型。
 - 单元类型的唯一实例等价于空元组。
 - 当元组只有一个元素的时候，要在元素末尾加逗号分隔这是为了方便和括号操作符区分开来。

## 1.7 Never 类型

 - 代表的是不可能返回值的计算类型。在类型理论中，叫做底类型。底类型不包含任何值，但它可以合一到任何其他类型。
 - Never 类型用「!」表示。
 - 目前还未文档，但是在 Rust 内部已经在用了。

# 2. 自定义复合类型

## 2.1 结构体（struct）

### 2.1.1 具名结构体

```rust
struct Point {
    x: f32,
    y: f32,
}
```

与 golang 不同，rust 编译器会自动对结构体进行内存对齐。如果不希望编译器优化，则需要增加`#[repr(C)]`属性，表示使用C语言结构体
布局。

```rust
#[repr(C)]
struct Point {
    x: f32,
    y: f32,
}
```

### 2.1.2 元组结构体

```rust
struct Score(u32);

impl Score {
    fn pass(&self) -> bool {
        self.0 >= 60
    }
}
```
元组结构体中的字段是没有字段名的，常用于包装某些基本类型。类似 Golang 中的 type。

```go
type Celsius float64
```

### 2.1.3 单元结构体

```rust
struct Unit;
```

单元结构体没有字段，单元结构体的实例就是它自身，仅做占位用，零大小类型。

## 2.2 枚举体（Enum）

```rust
enum E {
    N,
    H(u32),      // 函数项构造器
    M(Box<u32>), // 函数项构造器，Box 占8个字节，tag会补齐7个字节，一共16个字节
}
```
枚举体就是带tag的联合体，tag 可以理解为一种编号。tag 占一个字节。
枚举值也可以叫判别式。

## 2.3 联合体（Union）

```rust
union U {
    u: u32,
    v: u64, // 占8个字节
}
```

# 3. 容器类型

 - 可变容器
     - UnsafeCell
     - Cell
     - RefCell

 - 集合容器
     - Vec
     - HashMap
 
内部可变性：
 - 内部可变性与继承式可变相对应
 - 内部可变性由可变性核心原语 UnsafeCell<T>
 - 基于 UnsafeCell<T> 提供了 Cell<T> 和 RefCell<T>

## 3.1 Cell<T>

```rust
struct Foo {
    x: u32,
    y: Cell<u32>,
}


fn test_foo() {
    let foo = Foo {
        x: 1,
        y: Cell::new(3),
    };

    println!("{}", foo.y.get());

    foo.y.set(5);

    let x = Cell::new(5);
    let x1 = x.into_inner(); // 移出，MOVE
    ...
}
```

`Cell`的`set()`方法和`get()`方法可以用来修改和获取内部变量。`Cell`的`get()`只能用于实现了`Copy`的类型。如果没有实现，则
无法使用`get()`和`set()`方法。

## 3.2 RefCell<T>

适用于没有实现`Copy`的类型。

```rust
// 运行时借用检查
use std::cell::RefCell;

fn main() {
    let x = RefCell::new(vec![1, 2, 3, 4]);
    let mut mut_v = x.borrow_mut();

    mut_v.push(5);

    let mut mut_v2 = x.borrow_mut(); // 此处会 panic
}
```

# 4. 泛型

```rust
fn foo<T>(x: T) -> T {
    return x;
}

fn main() {
    assert_eq!(foo(1), 1);
    assert_eq!(foo("hello"), "hello");
}
```

Rust 中泛型会在编译期就单态化（即静态分发），所以泛型是零成本的。

```rust
foo(1);
// 等价于
foo::<i32>(1);

foo("hello");
// 等价于
foo::<&'static str>("hello");
```

`::<>`称之为 turbofish 操作符，用于给泛型函数指定具体类型，对于可以推断的类型，是可以省略的。

# 5. 特定类型

特性类型就是专门有特殊用途的类型。

1. PhantomData<T>，幻影类型。一般用于 Unsafe Rust 的安全抽象。

2. Pin<T>，固定类型。

为了支持异步开发而特意引进，防止被引用的值发生移动的类型。

