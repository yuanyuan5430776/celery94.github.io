---
layout: post
title:  "取2个已排序数组的前K个数"
date:   2016-03-01 21:00:00 +0800
categories: algorithms
---

已知两有序数组大小分别为m,n，获取前k个数，假设m＋n>k

**解答**

这是一个非常简单的题目，直需要三个指针变量，挨个遍历两个数组便可

{% highlight C# %}
public static int[] GetNumList(int[] nums1, int[] nums2, int k){
	int i = 0, m = 0, n = 0;
	var result = new int[k];
	for (; i < k; i++) {
		if (m < nums1.Length && n < nums2.Length) {
			if (nums1 [m] <= nums2 [n]) {
				result [i] = nums1 [m++];
			} else {
				result [i] = nums2 [n++];
			}
		} else if (m < nums1.Length) {
			result [i] = nums1 [m++];
		} else {
			result [i] = nums2 [n++];
		}
	}
	return result;
}
{% endhighlight %}
