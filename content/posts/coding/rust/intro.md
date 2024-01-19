+++
title = "Rust 学习笔记 | 介绍与安装"
date = "2020-01-11"
description = ""
tags = ["rust"]
categories = ["rust"]
toc = true
tocBorder = true
+++

## Rust 简介

Rust 的介绍，就不赘述了，还是去看 Wikipedia 上的吧 [Rust](https://zh.wikipedia.org/wiki/Rust)。官方网站以及文档资料可见：
- [Rust官方网站](https://www.rust-lang.org/)
- [Rust文档网](https://rustwiki.org/)
- [Rust设计模式](http://chuxiuhong.com/chuxiuhong-rust-patterns-zh/intro.html)
- [Rust语言圣经](https://course.rs/about-book.html)
- [Rust入门秘籍](https://rust-book.junmajinlong.com/)
- [Google 推出的入门教程](https://google.github.io/comprehensive-rust/welcome.html)

## Rust 环境初始化

### 使用 rustup 来管理版本
Rust 可以使用官方工具`rustup`来进行不同的版本管理，
官方网址 [https://rustup.rs/](https://rustup.rs/)，或者可以使用系统的包管理工具。
使用文档可以访问 [rustup-for-managing-rust-versions](https://rustwiki.org/zh-CN/edition-guide/rust-2018/rustup-for-managing-rust-versions.html)

```shell
# Macos
$ brew install rustup-init
# Arch linux
$ sudo pacman -S rustup
```

使用`rustup`安装 Rust

```bash
$ rustup install stable  # 安装 stable 版本
$ rustup install nightly # 安装 nightly 版本
$ rustup install 1.30.0  # 安装指定版本
$ rustup default stable  # 设置默认版本
```

> 如果需要更换为国内的安装源，请访问清华大学的[镜像源](https://mirrors.tuna.tsinghua.edu.cn/help/rustup/)和中科大的[镜像源](https://mirrors.ustc.edu.cn/help/rust-static.html)。

### 使用 cargo 管理项目
`rustup`是 rust 的版本管理器。`cargo`是 Rust 的构建系统和包管理器，比如构建代码、下载依赖库并编译这些库。一般安装 Rust 完成后，`cargo`已经安装完成。关于`cargo`的详细介绍：[Cargo-Book](https://doc.rust-lang.org/cargo/)。

 使用`cargo`初始化一个项目
```bash
# 创建新项目目录
$ cargo new new_project // --bin or --lib
# or
$ cargo init new_project
# cargo 命令会自动初始化一个 git 仓库
```

`new_project` 的结构如下：
```bash
.
├── Cargo.toml
└── src
└── main.rs
```

其中 `Cargo.toml` 的内容基本如下：
```toml
[package]
name = "new_project"
version = "0.1.0"
authors = ["username <username@xxx.com>"]
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html
[dependencies]
```

最后一行，`[dependencies]` 是罗列项目依赖的片段的开始。在 Rust 中，代码包被称为`crates`。Cargo 期望源文件存放在 `src` 目录中。
```bash
cargo build  # 构建项目使用命令
cargo build --release # 在优化模式下构建并生成可执行程序
cargo run    # 编译并运行
cargo check  # 检查代码但不编译
cargo test   # 运行测试
```

### cargo 的替代工具 cross
如果想要进行静态编译，减少依赖（比如libc.so、libssl.so等），编译的target需要设定为`x86_64-unknown-linux-musl`。
此时使用 cargo 工具就非常不方便。如果使用`musl`方式编译，需要安装各种依赖等。
此时可以使用 [cross](https://github.com/cross-rs/cross) 工具。

## Rust 开发工具以及其他小知识点

- neovim 如果想实现 rust 的语法补全和跳转，可以安装如下的工具：  
    - [coc.nvim](https://github.com/neoclide/coc.nvim) 插件以及插件的插件 [coc-rust-analyzer](https://github.com/fannheyward/coc-rust-analyzer)；
    - rust 的 LSP server 是 [rust-analyzer](https://github.com/rust-analyzer/rust-analyzer) ，可以使用`cargo`进行安装`cargo install ra_ap_rust-analyzer `。

- 安装 `cargo-edit` 来实现 `cargo add xxx` 来编辑项目目录下的 `cargo.toml`
  文件中的依赖项，安装`cargo install cargo-edit`。仓库链接：`https://github.com/killercup/cargo-edit`。
