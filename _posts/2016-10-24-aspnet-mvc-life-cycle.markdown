---
layout: post
title:  "ASP.NET MVC 生命周期"
date:   2016-10-24
tags: 
    - .net
    - asp.net
---

原文：[MVC Application Lifecycle](http://www.codeproject.com/Articles/741228/MVC-Application-Lifecycle)

## Introduction

这篇文件我们将要讨论MVC Application的生命周期和请求在各个模块的传递中是如何被处理的。
我们将要按在程序的生命周期中的顺序来讨论这些模块。
我们也将考虑每个组件以及它们与其他组件在管道中关系。

## Background

作为开发人员，我们都知道一些被MVC框架使用来处理请求的组件。
大部分时间我们都是在处理Controller和Action，同时也在处理不同的ActionResult和View。

但是，我们知道关于参与请求处理其他重要的组成部分么？这些请求在管道中是如何流动的？

当我开始学习MVC的时候，我不明白的是请求从一个组件如何流向另一个的。
此外，我也不清楚了HTTP module和Http handler在请求处理中扮演的角色。
毕竟MVC是一个Web开发框架，所以Http module和Http handler一定在管道中参与了处理。

有很多的模块参与了请求的处理管道，然后我们知道，controller和action参与了处理，这些也是在处理中的重要角色。

虽然大多数的时候我们可以使用该框架提供的默认功能，但如果我们了解为什么是为了可以方便地更换部件，或提供自定义实现组件。

## UrlRoutingModule

这是MVC应用程序的入口。

请求首先会被UrlRoutingModule截获，这是一个Http Module。
正是这种模块，决定请求是否会被MVC应用程序进行处理。
UrlRouting模块选择第一个匹配的路由。

UrlRoutingModule是如何匹配请求于路由的呢？
如果你看看从在Global.asax调用的RegisterRoutes方法，你会发现，这里将路由添加到路由RouteCollection。
这个方法是在Global.asax的Application_Start事件处理函数中调用的。
正是这个的RegisterRoutes方法，在注册应用程序的所有路由。

{% highlight C# %}
RouteConfig.RegisterRoutes(RouteTable.Routes);
public static void RegisterRoutes(RouteCollection routes)
{
    routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

    routes.MapRoute(
        name: "Default",
        url: "{controller}/{action}/{id}",
        defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }
    );
}
{% endhighlight %}

现在你可能会问的UrlRoutingModule是如何知道这些路由和路由如何与RouteHandler关联？
UrlRoutingModule使用MapRoute方法来获取关联。
如果你查看MapRoute方法，你会发现，它被定义为一个扩展方法。
在这里面关联了routerHanlder和routes，这个方法具体的实现是这样的：

{% highlight C# %}
var Default = new Route(url, defaults , routeHandler);
{% endhighlight %}

所以基本上这种方法就是附加routeHandler到route上。

UrlRoutingModule是这样定义的：
{% highlight C# %}
public class UrlRoutingModule : IHttpModule
{
    public UrlRoutingModule();
    public RouteCollection RouteCollection { get; set; }  //omitting the other details
}
{% endhighlight %}
