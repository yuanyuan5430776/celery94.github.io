---
layout: post
title:  "单向链表的翻转"
date:   2016-03-04 20:00:00 +0800
categories: algorithms
---

**题目**

单向链表的翻转

**解答**

主要的想法就是用三个指针，挨个遍历链表便可

{% highlight C# %}
public void Reverse(Node head){
	if (head == null)
		return null;

	Node pre = null;
	while (head != null) {
		var next = head.Next;
		head.Next = pre;
		pre = head;
		head = next;
	}

	return pre;
}
{% endhighlight %}