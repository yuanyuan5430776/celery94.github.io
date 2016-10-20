---
layout: post
title:  "Entity Framework Connection Management"
date:   2016-10-20
tags: 
    - .net
    - entity framework
---

## 关于传递Connection到Db Context

DbContext有多个构造函数，如果是以下几个，都将是创建新的数据库连接：

{% highlight C# %}
public DbContext(string nameOrConnectionString);
public DbContext(string nameOrConnectionString, DbCompiledModel model);
protected DbContext();
{% endhighlight %}

如果是采用以下的构造函数，将使用存在的数据库连接：

{% highlight C# %}
public DbContext(DbConnection existingConnection, bool contextOwnsConnection);
public DbContext(DbConnection existingConnection, DbCompiledModel model, bool contextOwnsConnection);
{% endhighlight %}

contextOwnsConnection是用来控制当前的DbContext释放之后，Connection是否关闭。

在EF6之前的版本中是有Bug的，contextOwnsConnection参数不一定生效，同时传递进来的数据库连接必须是Close的，
否者会因为再一次Open抛出异常InvalidOperationException。

## 关于Database.Connection.Open()

在EF6中，如果手动调用了context.Database.Connection.Open()，那么数据库连接将会一直在Open状态，必须手动调用Close才能关闭数据库连接。

Note: 当然这样可能会带来一定问题，数据库连接打开时间过长。

{% highlight C# %}
using (var context = new BloggingContext()) 
{ 
    // At this point the underlying store connection is closed 

    context.Database.Connection.Open(); 

    // Now the underlying store connection is open and the 
    // ObjectContext.Connection.State correctly reports open too 
        
    var blog = new Blog { /* Blog’s properties */ }; 
    context.Blogs.Add(blog); 
    context.SaveChanges(); 

    // The underlying store connection remains open for the next operation  
        
    blog = new Blog { /* Blog’s properties */ }; 
    context.Blogs.Add(blog); 
    context.SaveChanges(); 

    // The underlying store connection is still open 

} // The context is disposed – so now the underlying store connection is closed
{% endhighlight %}

在EF6之前的版本中，在第一个SaveChanges执行后，数据库连接将会是关闭状态（每一个数据库查询或者操作都将打开关闭数据库连接）。

## 关于数据库连接池

EF只是一个ORM框架，底层访问数据库还是使用的ADO.NET，所以同时也是使用的ADO.NET的数据库连接池。

由进程、应用程序域、连接字符串以及windows标识（在使用集成的安全性时）共同组成签名来标识区分的。
但对于同一应用程序域来说，一般只由连接字符串来标识区分。当打开一条连接时，如果该条连接的类型签名与现有的连接池类型不匹配，则创建一个新的连接池。
反之，则不创建新的连接池。

数据库连接池可以想象为一个存放多个数据库连接的地方，如果需要建立连接，从池里面去取，而不必开新的连接，起到缓存的作用。

* 如果池中有空闲连接可用，返回该连接。
* 如果池中连接都已用完，创建一个新连接添加到池中。
* 如果池中连接已达到最大连接数，请求进入等待队列直到有空闲连接可用。

如何配置数据库连接池：

* Connection Timeout：连接请求等待超时时间。默认为15秒，单位为秒。
* Max Pool Size: 连接池中最大连接数。默认为100。
* Min Pool Size: 连接池中最小连接数。默认为0。
* Pooling: 是否启用连接池。ADO.NET默认是启用连接池的，因此，你需要手动设置Pooling=false来禁用连接池。


