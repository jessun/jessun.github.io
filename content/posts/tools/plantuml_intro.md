+++
title = "工具介绍 | 绘图工具 PlantUML 介绍（WIP）"
date = "2018-01-01"
description = ""
tags = ["tools","uml"]
categories = ["tools"]
toc = true
tocBorder = true
draft = true
+++


## 介绍

PlantUML 是一个开源工具，能够让使用者通过以纯文本的方式来生成 UML 图（Unified Model Language 统一建模语言）。
通过PlantUML，使用者可以使用同一种风格来描述和生成各种图，如 UML 图、思维导图、甘特图等。
它的官方网址是 https://plantuml.com/zh/。

它的优点有：
- 高效。可以直接使用特定语法的文本进行编写，不需要使用 GUI 等可视化工具操作；
- 版本控制。由于使用纯文本编写，使用者可以用 git 等版本控制工具来管理文档的版本；
- 易于维护。后续维护，可以直接在原来的文本文档上做修改，无需重新绘制。
- 风格统一。


## 样例

```xml
!include https://ghproxy.com/https://raw.githubusercontent.com/jessun/plantuml_themes/dev/simple.puml

title <size:20> DMP 简单演示</size>

box "server-udp-1"
participant "umc-1"
participant "uagent-1"
participant "ustats-1"
participant "ucore-1"
database "mysql-udb"
end box

box "server-udp-2"
participant "uagent-2"
participant "ustats-2"
database "mysql-test-1"
end box

box "server-udp-3"
participant "uagent-3"
participant "ustats-3"
database "mysql-test-2"
end box

group umc 轮询获取实例健康状态
"umc-1"-\"ustats-1": ustats.GetStatsV2()
"ustats-1"[#blue]-/"umc-1": ustats.GetStatsV2()
end group

group umc 轮询获取状态
"umc-1"-\"ustats-2": ustats.GetStatsV2()
note right of "umc-1"
umc 向 ustats 请求获取实例状态，用于umc前端显示实例健康状态
end note
"ustats-2"[#blue]-/"umc-1": ustats.GetStatsV2()
note right of "umc-1"
end note
end group


group 
"umc-1"-\"ustats-3": ustats.GetStatsV2()
note right of "umc-1"
umc 向 ustats 请求获取实例状态，用于umc前端显示实例健康状态
end note
"ustats-3"[#blue]-/"umc-1": ustats.GetStatsV2()
note right of "umc-1"
end note
end group
```

## 语法介绍

推荐参考时序图的写法，https://plantuml.com/zh/sequence-diagram
其他类型，酌情使用

## 书写配置

1. Markdown 集成

vim/neovim 和 vscode 都支持在 markdown 文件中集成 PlantUML，可以直接渲染。

2. 后端服务

PlantUML 的图形绘制是使用一个 java 服务端来完成的。
线服务网址可访问 http://www.plantuml.com/plantuml/uml/。使用时，只需要将 PlantUML 文本复制进入即可获取到图片。
本地服务可以使用 docker 方式来运行`docker run -d -p 8080:8080 plantuml/plantuml-server:tomcat`，然后配置好本地的编辑器即可。

3. Vim/Neovim 编辑器配置

vim/neovim 编辑器需要配置插件`markdown-preview.nvim`，地址为`https://github.com/iamcco/markdown-preview.nvim`

其中主要配置为，uml 的 server 为 PlantUML server 服务的地址。

配置请参考 ![vim_config](./asserts/vim_config.png)

4. Vscode 编辑器

Vscode 可以完整支持 Markdown 文件中，内嵌 PlantUML 。

![vscode_demo](./asserts/vscode_demo.png)

Vscode 需要安装插件`PlantUML`，地址为 https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml

![vscode_plantuml](./asserts/vscode_plugin_plantuml.png)

配置请参考![vscode_plantuml_config](./asserts/vscode_config.png)

## 参考

- PlantUML 官方网址 https://plantuml.com/zh/
- vim/neovim 插件 https://github.com/iamcco/markdown-preview.nvim
- vscode plantuml 插件 https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml
