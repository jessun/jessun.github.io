+++
title = "工具介绍 | 绘图工具 PlantUML 介绍"
date = "2018-01-01"
description = ""
tags = ["tools","uml"]
categories = ["tools"]
toc = true
tocBorder = true
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

![](https://s2.loli.net/2023/05/19/hi24nojWX57bJmZ.png)

```plantuml
/' Nord 主题配色'/
!define NORD_POLAR_NIGHT1 #2E3440
!define NORD_POLAR_NIGHT2 #3B4252
!define NORD_POLAR_NIGHT3 #434C5E
!define NORD_POLAR_NIGHT4 #4C566A

!define NORD_SNOW_STORM1 #D8DEE9
!define NORD_SNOW_STORM2 #E5E9F0
!define NORD_SNOW_STORM3 #ECEFF4

!define NORD_FROST1 #8FBCBB
!define NORD_FROST2 #88C0D0
!define NORD_FROST3 #81A1C1
!define NORD_FROST4 #5E81AC

!define NORD_AURORA1 #BF616A
!define NORD_AURORA2 #D08770
!define NORD_AURORA3 #EBCB8B
!define NORD_AURORA4 #A3BE8C
!define NORD_AURORA5 #B48EAD

!define GRPC_REQ #BF616A
!define HTTP_REQ #D08770
!define RETURN #005BEA

<style>

note {
/'border 边框颜色'/  LineColor NORD_AURORA3 
/'border 边框粗细'/  LineThickness 1
/'背景色'/           BackGroundColor NORD_AURORA3
}

actor {
/'线条粗细'/   LineThickness 2
/'线条色'/     LineColor NORD_FROST4
/'背景色'/     BackgroundColor NORD_SNOW_STORM3
}

</style>

scale 10
skinparam dpi 96
skinparam responseMessageBelowArrow true
skinparam Participant {
/'背景色'/     BackgroundColor NORD_FROST4
/'边框色'/     BorderColor NORD_FROST4
/'字体颜色'/   FontColor NORD_SNOW_STORM3
}


skinparam Sequence {
/'Box 背景色'/    BoxBackgroundColor NORD_SNOW_STORM1
/'Box 边框色'/    BoxBorderColor NORD_SNOW_STORM3

/'箭头颜色'/      ArrowColor GRPC_REQ
/'箭头线条粗细'/  ArrowThickness 2

/'分隔线背景色'/    DividerBackgroundColor NORD_POLAR_NIGHT4
/'分隔线边框色'/    DividerBorderColor #eee
/'分割线边框粗细'/  DividerBorderThickness 0
/'分隔线字体颜色'/  DividerFontColor NORD_SNOW_STORM3
}

skinparam database {
/'背景色'/     BackgroundColor NORD_FROST1
/'边框色'/     BorderColor NORD_POLAR_NIGHT4
/'字体颜色'/   FontColor NORD_POLAR_NIGHT1
/'字体大小'/   FontSize 10
}


title <size:20> PlantUML 简单演示</size>

box "server_a"
participant "service_a"
database "mysql_a"
end box

box "server_b"
participant "service_b"
database "mysql_b"
end box


box "server_c"
participant "service_c"
database "mysql_c"
end box

service_a -> service_b
service_b -> service_c
service_b <- service_c
service_a <- service_b

```


## 语法介绍

推荐参考时序图的写法，https://plantuml.com/zh/sequence-diagram。

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

配置请参考 ![](https://s2.loli.net/2023/05/19/TNuCwJMZbsmFHeI.png)

4. Vscode 编辑器

Vscode 可以完整支持 Markdown 文件中，内嵌 PlantUML 。
![](https://s2.loli.net/2023/05/19/oipPEmDb8tNgGL3.png)

Vscode 需要安装插件`PlantUML`，地址为 https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml
![](https://s2.loli.net/2023/05/19/YkmwvFuMyRigNq5.png)

配置请参考
![](https://s2.loli.net/2023/05/19/akt2lG8OJHxBCmI.png)

## 参考

- PlantUML 官方网址 https://plantuml.com/zh/
- vim/neovim 插件 https://github.com/iamcco/markdown-preview.nvim
- vscode plantuml 插件 https://marketplace.visualstudio.com/items?itemName=jebbs.plantuml
