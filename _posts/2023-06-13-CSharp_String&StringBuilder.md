---
layout: post
usemathjax: false
title: CSharp_String&StringBuilder
date: 2023-06-13
tags: algorithm csharp
---

<!-- <span style="color: blue;"> </span> -->
# String & StringBuilder 

> 虽然在 <mark>c#</mark> 中 **String** 和 **StringBuilder** 都是表示字符串的类，**但是他们底层的运作机制是不一样的！**

<!--more-->
- String 是一个不可修改的类型，所有看起来是修改了String对象的操作，其实是创建了一个新的String对象，比如我们所熟知的连结操作 **String.Concat(str1, str2)**，他所返回的地址和原本 str1 或 str2 的地址是不一样的。
- StringBuilder 则是可修改的字符串类型，可以对一个StringBuilder对象进行 append, remove, replace 或者 insert 操作，他其实是在相应的 buffer 区域对字符串进行修改的，如果一个修改操作以后的长度大于 buffer 空间了 或者说 **没有足够的区域提供该修改操作了**，将会申请新的 buffer 区，并将原来 buffer 区中的数据复制到新的 buffer 区中，然后再进行相应的修改。

> 什么时候用 String? 什么时候用 StringBuilder?

- String
  - 当修改操作较少的时候，推荐使用String。
  - 当字符串的连接操作是确定的时候，推荐使用String，因为编译器会识别并将多个连接操作组合成一个操作。
  - 当需要大量的查询操作的时候，推荐使用String，其中自带相应的方法，比如 **IndexOf(), StartsWith()** 等。
- StringBuilder
  - 当你需要对一个字符串对象进行多次修改操作的时候。
  - 当你需要接受用户输入对字符串对象进行修改的时候。

**一句话总结：当需要对字符串进行多次修改的时候，推荐使用 StringBuilder，当需要多次查询时，，推荐使用 String**

> StringBuilder 底层机制

- 相比String，StringBuilder 对象多一个 **capacity** 的特性，表明对象最多能存储几个字符，如果有比这个更长的字符被加入到StringBuilder 对象中，会触发扩容的机制，即重新申请新的一块空间，capacity更改为原来的两倍，再将多出来的字符加入到新申请的Buffer中，注意**即使要发生扩容，那么我当前节点的 Buffer 也一定要填充满，新的 Buffer 存多余出来的字符串，保证了存储空间的最大利用。**
- 扩容的操作本质是 **构建节点为StringBuilder的单向链表** 的操作，上一个 Buffer 的地址信息保留在以下成员中：

```c#
internal StringBuilder? m_ChunkPrevious;
``` 

- 同时不能无限扩容，有一个 MaxCapacity 控制最大容量

大致结构如下图：

![c_sharp_stringbuilder1]({{site.baseurl}}/assets/img/c_sharp_stringbuilder1.png)

- 栈空间中存储的 StringBuilder 对象的地址是最后一个StringBuilder Buffer 块的地址，**同时根据这个单链表的结构我们也可以知道查询操作对于 StringBuilder 来说很费事**，所以一般对字符串有查询操作的推荐使用String，如下图：
  
![c_sharp_stringbuilder2]({{site.baseurl}}/assets/img/c_sharp_stringbuilder2.png)

**注意，上图中的字符串常量池在 c# 不存在，可以直接忽视，c# 中没有 String Constant Pool 的概念，只有类似的 Intern Pool，可自行查阅**

