+++
title = "Rust 学习笔记 | cargo 工具介绍"
date = "2024-03-07"
description = ""
tags = ["rust"]
categories = ["rust"]
toc = true
tocBorder = true
draft = true
+++

## `cargo` 简介

`cargo`是rust的包管理工具,可以用来下载rust的包依赖、
编译packages包、生成可分发的packages包、上传packages包至[crates.io](https://crates.io/)。

## 使用`cargo`初始化项目

```sh
cargo new project_a # 初始化一个rust项目
```
该操作会：
- 新建一个名为`project_a`的目录，并且已经初始化为git仓库
