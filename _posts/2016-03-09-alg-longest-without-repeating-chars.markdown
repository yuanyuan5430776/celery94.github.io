---
layout: post
title:  "Longest Substring Without Repeating Characters - Leetcode No.3"
date:   2016-03-09
tags: 
    - algorithms
---

Given a string, find the length of the longest substring without repeating characters. 

For example, the longest substring without repeating letters for "abcabcbb" is "abc", which the length is 3. 

For "bbbbb" the longest substring is "b", with the length of 1.

**解答**

用Hashtable是处理这种问题，同时使用index作为不重复字符串的开始指针，如果发现了重复字符，将index移动到重复字符串的后一位。

{% highlight C# %}
public int LengthOfLongestSubstring (string s){
	int max = 0;
	int index = 0;
	Hashtable hash = new Hashtable();
	for (int i = 0; i < s.Length; i++) {
		if (hash.Contains (s[i])) {
			if ((int)hash [s[i]] >= index) {
				index = (int)hash [s [i]] + 1;
			}
			hash[s[i]] = i;
		} else {
			hash.Add (s[i], i);
		}

		var length = i - index + 1;
		if (max < length) {
			max = length;
		}
	}

	return max;
}
{% endhighlight %}