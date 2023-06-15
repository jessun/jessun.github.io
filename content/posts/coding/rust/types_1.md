+++
title = "Rust 学习笔记 | 类型综述"
date = "2020-07-01"
description = ""
tags = ["rust"]
categories = ["rust"]
toc = true
tocBorder = true
draft = false
+++

## Rust 所有的类型梳理

- 基本类型（Primitive Types）
    - 布尔类型，bool，Copy
    - 字符类型，char，Copy
    - 数值类型
        - 整数类型，i8/i16/i32/i64/i128/isize，Copy
        - 无符号整数类型，u8/u16/u32/u64/u128/usize，Copy
        - 浮点数类型，f32/f64，Copy

- 复合类型（Compound Types）
    - 元组类型，tuple。如果成员都是 Copy 的，那么该元组就是 Copy ；
    - 数组类型，array。如果成员都是 Copy 的，那么该数组就是 Copy 的
    - 结构体类型，struct。如果成员都是 Copy 的，那么该结构体就是 Copy 的
    - 切片类型，slice，非 Copy
    - 动态数组类型，Vec<T>，非 Copy

- 枚举类型（Enumeration Types）
    - 枚举类型，enum。如果所有变体都是 Copy 的，那么该枚举类型就是 Copy 的
    - Option 类型，如果被包裹的类型就是 Copy 的，那么该 Option 就是 Copy 的

- 引用类型（Reference Types）
    - 引用，&T，非 Copy

- 指针类型（Pointer Types）
    - 裸指针，*const T/*mut T，非 Copy
    - 函数指针，fn，非 Copy

- 字符串类型（String Types）
    - 字符串切片，&str，非 Copy
    - 字符串类型，String，非 Copy

## Copy 检查
如何检查一个结构体是否是 Copy 的？
```rust
#[derive(Clone, Copy)]
struct MyStruct {
    // 结构体的字段
    a: i32,
}

fn is_copy<T: Copy>() {
    // 不执行任何操作，仅用于检查编译是否通过
}

fn main() {
    if std::panic::catch_unwind(|| {
        is_copy::<MyStruct>();
    })
    .is_ok()
    {
        println!("MyStruct is Copy");
    } else {
        println!("MyStruct is not Copy");
    }
}
```
