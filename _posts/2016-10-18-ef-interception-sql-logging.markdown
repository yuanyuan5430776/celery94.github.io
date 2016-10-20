---
layout: post
title:  "EF Interception/SQL logging"
date:   2016-10-18
tags: 
    - .net
    - entity framework
---

Entity framework Interception and SQL logging是在EF6.0之后新增的功能，能够非常方便查看SQL执行时间。
下面是一个简单例子，在日志中显示执行时间超过1s的sql。

配置文件，在app.config或者web.config的entityframework节点添加：

{% highlight xml %}
  <entityFramework>
    <interceptors>
      <interceptor type="EFSample.EFLogger, EFSample">
        <parameters>
          <parameter value="1000" type="System.Int32"/>
        </parameters>
      </interceptor>
    </interceptors>
  </entityFramework>
{% endhighlight %}

拦截器的实际实现：

{% highlight C# %}
namespace EFSample
{
    public class EFLogger : IDbCommandInterceptor
    {
        static readonly ConcurrentDictionary<DbCommand, DateTime> MStartTime = new ConcurrentDictionary<DbCommand, DateTime>();
        static int _interval;

        public EFLogger(int interval)
        {
            _interval = interval;
        }

        public void NonQueryExecuting(DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
        {
            OnStart(command);
        }

        public void NonQueryExecuted(DbCommand command, DbCommandInterceptionContext<int> interceptionContext)
        {
            Log(command, interceptionContext);
        }

        public void ReaderExecuting(DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
        {
            OnStart(command);
        }

        public void ReaderExecuted(DbCommand command, DbCommandInterceptionContext<DbDataReader> interceptionContext)
        {
            Log(command, interceptionContext);
        }

        public void ScalarExecuting(DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
        {
            OnStart(command);
        }

        public void ScalarExecuted(DbCommand command, DbCommandInterceptionContext<object> interceptionContext)
        {
            Log(command, interceptionContext);
        }

        private static void OnStart(DbCommand command)
        {
            MStartTime.TryAdd(command, DateTime.Now);
        }

        private static void Log<T>(DbCommand command, DbCommandInterceptionContext<T> interceptionContext)
        {
            DateTime startTime;
            TimeSpan duration;
            MStartTime.TryRemove(command, out startTime);
            if (startTime != default(DateTime))
            {
                duration = DateTime.Now - startTime;
            }
            else
                duration = TimeSpan.Zero;

            var cmdText = command.CommandText;
            var parameters = new StringBuilder();
            foreach (DbParameter param in command.Parameters)
            {
                cmdText = cmdText.Replace($"@{param.ParameterName}", param.Value.ToString());
            }

            if (interceptionContext.Exception != null)
            {
                Logger.WriteError("", interceptionContext.Exception);
            }

            if (duration.TotalMilliseconds >= _interval)
            {
                Logger.WriteDebug($"Duration TotalMilliseconds:{duration.TotalMilliseconds}  SQL:{cmdText}");
            }
        }
    }
}
{% endhighlight %}
