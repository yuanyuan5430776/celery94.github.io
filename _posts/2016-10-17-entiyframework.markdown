---
layout: post
title:  "About entity framework"
date:   2016-10-17
tags: 
    - .net
---

## Entity Framework是什么
EF是一个微软以ADO.NET为基础发展起来的的ORM（Object-Relational Mapping）框架。

EF将数据库的表都转换成应用程序对象（entity）,数据字段都转换为属性（property）,关系转换为结合属性（association）。

## EF4，5，6各版本对比
EF5 新增的功能：

* 枚举的支持，在EF设计器和Code First中是可用的
* 可以将数据库中的表值函数导入

EF6 新增的功能：

* Async Query and Save
* Interception/SQL logging: 非常实用的功能，可以用来记录执行时间较长的数据库操作，[示例TODO]()
* Code First Insert/Update/Delete Stored Procedure Mapping

效率的提高，详细的测试报告在这里：

[Performance Considerations for Entity Framework 4, 5, and 6](https://msdn.microsoft.com/en-us/data/hh949853(v=vs.113).aspx#10)

## Code Fisrt vs DB First
Code First的特点：

* 不用使用过于复杂的EDMX XML配置文件
* 没有自动生成的代码
* 不用考虑数据库，数据库只是用来存储，EF能够自动创建数据库
* 可能会丢失手动更改的数据库

DB First的特点：

* 非常适合在数据库已经设计好的情况
* EF能够生成现成的POCO
* 可以通过修改T4模板来给POCO新增功能，或者使用parial class
* 能够手动修改数据库而不会丢失更改

## ObjectContext vs DbContext
在EF中，会生成context class来实现数据库的访问。

在EF4.1中，context class是从ObjectContext派生而来的。

在EF6.0中，context class是从DbContext派生的。

DbContext是在ObjectContext的基础上实现的，能够更好的使用在EF的三种开发模式中。

DbContext在通常情况比ObjectContext要更简单实用，同时也能够通过DbContext获取到ObjectContext。

{% highlight C# %}
using (var ctx = new DBEntities())
{
    var objectContext = (ctx as System.Data.Entity.Infrastructure.IObjectContextAdapter).ObjectContext;
        
    //use objectContext here..

}
{% endhighlight %}
