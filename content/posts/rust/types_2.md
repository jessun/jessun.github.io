+++
title = "Rust 学习笔记 | 数组、动态数组和 HashMap"
date = "2020-07-01"
description = ""
tags = ["rust"]
categories = ["rust"]
toc = true
tocBorder = true
draft = false
+++

## 数组（array）

### 概念要点

- Rust 数组的概念与 Golang 中的类似；
- 数组的长度是**固定的**，长度不同的数组不是一个类型；
- 数组中的元素类型都必须是相同的。如果有需要，可以使用枚举类型或者特征对象来处理。

### 数组的相关操作

数组的创建
```rust
let int_array = [9, 8, 7, 6, 5];
let str_array: [String; 8] = std::array::from_fn(|i| String::from("rust is good!"));
```

访问数组元素
```rust
// 方式一：使用索引来直接访问元素。如果索引超过了数组长度，那么会出现 panic
int_array[2]

// 方式二
int_array.get(index)

// 遍历方式一
for item in int_array.iter() {
// ...
}

// 遍历方式二
for (index, item) in int_array.iter().enumerate() {
    // ...
}								
```

### 数组切片

```rust
let a: [i32; 5] = [1, 2, 3, 4, 5];
let slice: &[i32] = &a[1..3];
```

Rust 中数组切片的概念类似 Golang，都是对数组的片段引用。
切片长度可以与数组不同，并不固定。
切片类型[T]的大小是不固定的，而切片引用类型&[T]的大小是固定。
实际编码中，&[T]拥有固定的大小，因此使用更普遍，比如 &str。

## 动态数组类型 Vec<T>

### 概念要点

- 动态数组类型用`Vec<T>`表示。

### 动态数组的相关操作

创建动态数组
```rust
// 方式一
let v: Vec<i32> = Vec::new(); 

// 方式二
let mut v = Vec::new(); 
v.push(1);

// 方式三
let v = vec![1, 2, 3];

// 其他
let a: Vec<i32> = Vec::with_capacity(5); // 创建指定容量的动态数组，可以避免频繁的内存分配和拷贝
```

更新动态数组
```rust
let mut v = Vec::new();
v.push(1);
```

访问动态数组中的元素
```rust
let v = vec![1, 2, 3, 4, 5];

// 方式一，可能存在访问越界的问题
let third: &i32 = &v[2];
println!("第三个元素是 {}", third);

// 方式二
match v.get(2) {
    Some(third) => println!("第三个元素是 {third}"),
    None => println!("去你的第三个元素，根本没有！"),
}
```

访问不同类型的元素

使用枚举类型来实现；
```rust
enum IpAddr {
    V4(String),
    V6(String)
}

let v = vec![
    IpAddr::V4("127.0.0.1".to_string()),
    IpAddr::V6("::1".to_string())
];
```

使用特征对象来实现；
```rust
trait IpAddr {
    fn display(&self);
}

let v: Vec<Box<dyn IpAddr>> = vec![
// 不同的特征对象
];

```

## KV 存储 HashMap

### 概念要点

- HashMap 类似 Golang 中的 map；

### HashMap 的相关操作

HashMap 的创建
```rust

use std::collections::HashMap;

// 方式一
let mut my_gems: HashMap<&str, i32> = HashMap::new();

// 方式二
let mut my_gems = HashMap::new();
my_gems.insert("红宝石", 1);

// 方式三：将Vec转为迭代器，结果使用collect方法转为HashMap
let teams_list = vec![
    ("中国队".to_string(), 100),
    ("美国队".to_string(), 10),
    ("日本队".to_string(), 50),
];

let teams_map: HashMap<_,_> = teams_list.into_iter().collect();
```

访问 HashMap 中的元素
```rust
let score: Option<&i32> = scores.get(&team_name);
```

HashMap 的更新
```rust
// 更新方式一：直接覆盖
let old = scores.insert("Blue", 20);

// 更新方式二：
let v = scores.entry("Yellow").or_insert(5);

```

### 哈希函数

Rust HashMap 使用的哈希函数是`SipHash`，性能不是很高，但是安全性很好。
可以根据具体的场景需要更改哈希函数。
