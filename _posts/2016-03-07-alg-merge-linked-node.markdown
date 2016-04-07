---
layout: post
title:  "链表合并"
date:   2016-03-05
categories: algorithms
---

已知两有序链表，求合并后链表

**解答**

递归解决方案：

{% highlight C# %}
public Node Merge (Node l1, Node l2)
{
   if (l1 == null && l2 == null)
      return null;

   Node head;
   if (l1 == null)
      return l2;
   if (l2 == null)
      return l1;

   if (l1.Val <= l2.Val) {
      head = l1;
      head.Next = Merge (l1.Next, l2);
   } else {
      head = l2;
      head.Next = Merge (l1, l2.Next);
   }

   return head;
} 
{% endhighlight %}
