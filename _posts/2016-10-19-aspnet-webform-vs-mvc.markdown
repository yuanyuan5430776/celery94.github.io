---
layout: post
title:  "ASP.NET Web Form Compare with ASP.NET MVC"
date:   2016-10-19
tags: 
    - Other
---

## ASP.NET Web Form 的特点 （快速开发，学习成本低）

* 使用内置的服务器控件，最终会生成HTML和Javascript，可能会出现浏览器的兼容问题，同时不能完全控制页面的HTML，集成jQuery困难
* 使用ViewState保持表单的中间状态, 增加了页面的大小，降低了性能
* 事件驱动的编程方式
* 单元测试很难介入，可重用性差
* 页面的URL为物理文件的路径

## ASP.NET MVC 的特点

对比如Web Form，MVC有很多优点：

* 优良的可测试性
* 性能优于Web Form
* 完全的HTML控制，方便集成javascript库
* SEO, URL重写和REST支持
* 支持多种视图引擎：aspx，razor

## ASP.NET MVC 是如何工作

* Model，传统的Model包含数据和获取数据，包含业务逻辑，但是在这边只是数据容器
* View，数据模型的输出表现，可以理解为页面
* Controller，接受用户的请求和交付，最终将View输出

* 用户通过浏览器URL向服务器请求一些资源。（GET请求）
* 路由引擎首先将这个请求传递给对应的Controller。
* 如果需要持久化数据，Controller就从Model中获得数据。
* Model访问数据库数据（或其它一些数据资源）并返回数据给Controller。
* Controller将选择适当的View。
* Controller将数据（第四部获取的model）传递给页面（第五部获取的view），数据将填充到页面的指定位置。
* Controller将View返回并呈现给用户。

