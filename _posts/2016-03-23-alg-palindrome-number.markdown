---
layout: post
title:  "Palindrome Number - Leetcode No.9"
date:   2016-03-23
categories: algorithms
---

**题目**

Determine whether an integer is a palindrome. 

_Do this without extra space._

**解答**

这个解法使用了额外的存储空间，将输入反转了之后来看是否和原来相等。

{% highlight C# %}
public bool IsPalindrome(int x) {
    var y = x;
    var ret = 0;
    while(x > 0){
        ret = ret * 10 + x % 10;
        x = x/10;
    }
    
    return ret == y;
}
{% endhighlight %}