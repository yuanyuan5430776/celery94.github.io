---
layout: post
title:  "RPC, Webservice, REST对比"
date:   2016-10-15
tags: 
    - Other
---

## RPC是什么：
RPC的全称是Remote Procedure Call，字面意思就是远程过程调用，这是一个计算机通信协议，这个协议允许一台计算机的程序调用另一台计算机的程序。
实现了这个协议的框架有很多，比如Java RMI，Web Service，RESTful Wev Services等等。

## Web Service是什么：
Web Service作为一个RPC框架，要实现的功能也是支持远程调用，它通过标准的Web协议提供服务，可以在各平台中互相操作。
在Web Serivice框架中，定义一些标准：

* __SOAP（Simple Object Access Protocal__
即简单对象访问协议，这个是一个交换数据的协议规范。
SOAP数据一般使用HTTP或者HTTPS来传输，也支持其它的传输协议，比如SMTP，只是应用的很少。
SOAP的消息格式采用的是XML。

* __WSDL (Web Services Description Language)__
即Web服务描述语言，这是用来描述Web Service的公共接口，也是基于XML。

## REST是什么：
REST的全称是Representational State Transfer，可以翻译为具象状态传输，或者表述行状态转移。不过随便怎么翻译估计都没人能看懂这个是啥意思。
REST其实只是一个架构风格，与Web Service比起来更加简洁。
RESTful Web Service（Web API）是一个使用HTTP并且符合REST风格的Web服务。

主要特征：

* 面向资源，每一个URI代表一种资源
* 客户端和服务器之间传递
* 客户端通过HTTP的四个动词，对服务器端资源进行操作


## WCF是什么
微软发展的一组数据通信的应用程序开发接口。
相当于进化版的Web Service，支持的协议很多，比如TCP，HTTP，HTTPS，Named Pipes，MSMQ。最初也是基于SOAP的，数据格式是XML。
WCF在后期添加了对REST迎合的功能，在WCF4.5有很大的增强，可以叫做WCF Web API。

## ASP.NET Web API
ASP.NET Web API是一个在.net framework上专门用来创建RESTful项目的。

## WCF和ASP.NET Web API应该怎么选择

* 当你想创建一个支持消息、消息队列、双工通信的服务时，你应该选择WCF

* 当你想创建一个服务，可以用更快速的传输通道时，像TCP、Named Pipes或者甚至是UDP（在WCF4.5中）,在其他传输通道不可用的时候也可以支持HTTP时，你应该选择WCF。

* 当你想创建一个基于HTTP的面向资源的服务并且可以使用HTTP的全部特征时（比如URIs、request/response头，缓存，版本控制，多种内容格式），你应该选择Web API

* 当你想让你的服务用于浏览器、手机、iPhone和平板电脑时，你应该选择Web API