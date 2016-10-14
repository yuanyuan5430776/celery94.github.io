---
layout: post
title:  "Container With Most Water - Leetcode No.11"
date:   2016-03-25
tags: 
    - algorithms
---

Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). 

Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container.

**解答**

才用左右双指针的方式，那一边小，那一边就继续往中间移动，找到比当前更大的数值，挨个计算面积。

{% highlight C# %}
public int MaxArea(int[] height) {
    int left = 0, right = height.Length -1;
    
    var max = 0;
    while(left< right){
        
        var h= height[left] > height[right]?height[right]:height[left];
        var area = (right - left) * h;
        
        if(area > max){
            max = area;
        }
        
        if(height[left] > height[right])
        {
            while(left < right && height[right] <= h){
                right--;
            }
        }
        else{
            while(left < right && height[left] <=h){
                left ++;
            }
        }
    }
    
    return max;
}
{% endhighlight %}