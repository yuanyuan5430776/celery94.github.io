---
layout: post
title:  "Two Sum - Leetcode No.1"
date:   2016-03-08
categories: algorithms
---

Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution.

**解答**

用Hashtable是处理这种问题最简单的方案，只是需要额外的空间。

{% highlight C# %}
public int[] TwoSum(int[] nums, int target) {
   Hashtable hash = new Hashtable();
   for(int i =0;i<nums.Length;i++){
      if(hash.Contains(target - nums[i])){
         return new int[]{i,(int)hash[target - nums[i]]};
      }
      if(!hash.Contains(nums[i]))
         hash.Add(nums[i],i);
   }

   return new int[]{};
}
{% endhighlight %}