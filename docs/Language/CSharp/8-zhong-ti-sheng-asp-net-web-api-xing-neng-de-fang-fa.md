---
layout: default
title: 提升ASP.NET Web API性能方法
nav_order: 4
parent: CSharp
grand_parent: Language
permalink: /docs/Language/CSharp
---

# 8 种提升 ASP.NET Web API 性能的方法

FROM [开源中国](https://www.oschina.net/translate/8-ways-improve-asp-net-web-api-performance)

英文原文：[8 ways to improve ASP.NET Web API performance](http://blog.developers.ba/8-ways-improve-asp-net-web-api-performance/)

ASP.NET Web API 是非常棒的技术。编写 Web API 十分容易，以致于很多开发者没有在应用程序结构设计上花时间来获得很好的执行性能。

在本文中，我将介绍8项提高 ASP.NET Web API 性能的技术。

# 1\) 使用最快的 JSON 序列化工具 {#content_h1_1_7}

JSON 的序列化对整个 ASP.NET Web API 的性能有着关键性的影响。 在我的一个项目里，我从 JSON.NET 序列化工具转到了 [ServiceStack.Text](https://servicestack.net/text) 有一年半了。

我测量过，Web API 的性能提升了20%左右。 我强烈建议你去尝试一下这个序列化工具。这里有一些最近的流行序列化工具性能的比较数据。

![/assets/404.png](/assets/1008239.png)

[来源： theburningmonk](http://theburningmonk.com/2014/06/json-and-binary-serializers-benchmarks-updated/)

更新: 似乎It seams that StackOverflow 使用了他们号称迄今为止最快的 JSON 序列化工具_**Jil**_。 一测试数据可参见他们的GitHub page[Jil serializer](https://github.com/kevin-montrose/Jil).

# 2\)从DataReader中手动串行化JSON {#content_h1_2_5}

我已经在我的项目中使用过这种方法，并获得了在性能上的福利。

你可以手动地从DataReader创建JSON字符串并避免不必要的对象创建，这样你就不用从DataReader中取值并写入对象，再从这些对象中取值并使用JSON Serializer产生JSON.

使用StringBuilder产生JSON，并在结尾处返回StringContent作为在WebAPI中响应的内容。

``` C#
var response = Request.CreateResponse(HttpStatusCode.OK);
response.Content = new StringContent(jsonResult, Encoding.UTF8, "application/json");
return response;
```

你可以在 [Rick Strahl’s blog](http://weblog.west-wind.com/posts/2009/Apr/24/JSON-Serialization-of-a-DataReader)查看更多方法

# 3\)尽可能使用其它协议格式 \(protocol buffer, message pack\) {#content_h1_3_5}

如果你能给在你的工程中使用其它消息格式，如_Protocol Buffers _或_MessagePack_而不是使用JSON这种协议格式。

你将能给获取到巨大的性能优势，不仅是因为_Protocol Buffers_的序列化是非常快,而且比JSON在返回的结果格式化要更快。

# 4\) 实现压缩 {#content_h1_3_15}

在你的ASP.NET Web API中使用_GZIP_或 _Deflate 。_

对于减少响应包的大小和响应速度，压缩是一种简单而有效的方式。

这是一个非常有必要使用的功能，你可以查看更多关于压缩的文章在我的博客 [ASP.NET Web API GZip compression ActionFilter with 8 lines of code](http://blog.developers.ba/asp-net-web-api-gzip-compression-actionfilter/).

# 5\) 使用caching {#content_h1_4_5}

在Web API方法中使用_output caching_意义深远.举例来说,如果大量用户访问同一个一天只改变一次的响应\(response\)内容。

如果你想实现手动缓存，例如把用户口令缓存到内存，请参看我的博文 [Simple way to implement caching in ASP.NET Web API](http://blog.developers.ba/simple-way-implement-caching-asp-net-web-api/).

# 6\) 尽可能地使用典型的 ADO.NET {#content_h1_4_10}

手动编写的ADO.NET仍然是从数据库中取值的最快捷的方式。如果Web API的性能对你来说真的很重要，那么就不要使用ORMs.

你可以看到最流行的ORM之间的性能比较.

![/assets/404.png](/assets/1008237.png)

_Dapper_和_hand-written fetch code_很快，果不其然，所有的ORM都比这三种慢.

_带有resultset缓存的LLBLGen_ 很快，但它要重新遍历一遍resultset并重新再内存中实例化对象。

# 7\)在 Web API 中实现异步方法 {#content_h1_5_5}

使用异步的 Web API 服务大幅增加 Web API 对于Http 请求的处理数量。

实现是简单的，只需使用 _async  的关键字和 将你方法的返回值类型改为 Task即可。_

``` C#
[HttpGet]  
public async Task OperationAsync()  
{
    await Task.Delay(2000);  
}
```

# 8\) 返回多个结果集和集合的组合 {#content_h1_6_5}

减少传输的次数不仅多数据库有好处，对于 Web API同样 ，你才有可能使用结果集的功能。

也就是说你可以从**DataReader**去提取多个结果集 参见以下演示代码：

``` C#
// read the first resultset
var reader = command.ExecuteReader();

// read the data from that resultset
while (reader.Read())
{
    suppliers.Add(PopulateSupplierFromIDataReader( reader ));
}

// read the next resultset
reader.NextResult();

// read the data from that second resultset
while (reader.Read())
{
    products.Add(PopulateProductFromIDataReader( reader ));
}
```

你可以在一个 Web API 的一次响应中返回多个对象，试着将你的返回的多个对象进行组合后返回 如下：

``` C#
public class AggregateResult
{
     public long MaxId { get; set; }
     public List<Folder> Folders{ get; set; }
     public List<User>  Users{ get; set; }
}
```

这种方式将减少对你的WEB API的HTTP请求。