---
layout: post
title:  "字符串反转"
date:   2016-03-07
categories: algorithms
---

**题目**

实现字符串反转函数，例如，"July"反转后变成"yluJ"

**解答**

{% highlight C# %}
public string StringReverse(string input){
   var arr = input.ToCharArray ();

   int left = 0, right = arr.Length - 1;
   while (left < right) {
      var temp = arr [left];
      arr [left] = arr [right];
      arr [right] = temp;
      left++;
      right--;
   }

   return string.Join ("", arr);
}
{% endhighlight %}