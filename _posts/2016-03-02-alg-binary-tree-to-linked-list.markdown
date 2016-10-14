---
layout: post
title:  "转换二叉树为双向链表"
date:   2016-03-02 21:00:00 +0800
categories: algorithms
---

转换二叉树为双向链表

**解答**

采用中序遍历二叉树，同时使用pre node作为current node的前节点，大概的代码如下

{% highlight C# %}
private Node pre = null;
public void Convert(Node node){
	if (node != null) {
		Convert (node.Left);

		if (pre != null) {
			pre.Right = node;
			node.Left = pre;
		}

		pre = node;

		Convert (node.Right);
	}
}
{% endhighlight %}