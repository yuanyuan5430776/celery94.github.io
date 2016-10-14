---
layout: post
title:  "数组排序为负数,0,正数(荷兰国旗问题🇳🇱，颜色排序问题)"
date:   2016-03-04 12:00:00 +0800
tags: 
    - algorithms
---

已知数组为[2,0,-1,5,4,3,-6,0,6],将数组排序为负数在最前，正数在后半部。

**解答**

这个题目还有一个更有名的题目叫荷兰国旗问题，因为荷兰国旗只有三个颜色，或者也叫三色球排序问题。

思路并不复杂，使用三个指针，begin，current，end，current指针挨个往后遍历，

如果发现是负数，begin和current指针交换值，begin++，current++

如果发现是正数，end和current指针交换值，end--

一直到current==end时结束

{% highlight C# %}
while( current<=end )        
{             
  if( array[current] ==0 )             
   {                 
      swap(array[current],array[begin]);                  
      current++;                  
      begin++;            
   }             
   else if( array[current] == 1 )            
   {                 
      current++;            
   }   

   else //When array[current] =2   
   {               
      swap(array[current],array[end]);                
      end--;            
   }      
}  
{% endhighlight %}