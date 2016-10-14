---
layout: post
title:  "Reverse Integer - Leetcode No.7"
date:   2016-03-14
tags: 
    - algorithms
---

Reverse digits of an integer.

Example1: x = 123, return 321

Example2: x = -123, return -321

**解答**

需要考虑负数和溢出问题。

{% highlight C# %}
public int Reverse(int x) {
    bool flag = x<0;
    if(flag) x = x*-1;
    
    var ret = 0;
    while( x > 0){
        var m = x % 10;
        x= x/ 10;
        
        if((int.MaxValue - m)/10<ret){
         	return 0;
        }
        ret = ret * 10 +m;
    }
    
    return flag?(0-ret): ret;
}
{% endhighlight %}