---
layout: post
title:  "求连续正整数和为n的所有组合"
date:   2016-02-28 21:00:00 +0800
tags: 
    - algorithms
---

给定一个正整数n，求任意连续整数，该连续整数的和为给定的正整数n

**解法1**

穷举肯定是可以解决改问题的，大概的方法就是用i，j两个数做双重循环，取i到j的中间全部整数，如果和为n就是要的连续数组了。

大概的代码如下

{% highlight C# %}
public static List<List<int>> GetNums(int n){
	List<List<int>> list = new List<List<int>> ();
	for (int i = 1; i <= n / 2; i++) {
		for (int j = i + 1; j <= n/2 + 1 ; j++) {
			var sum = Sum (i , j);
			if (sum == n) {
				List<int> item = new List<int>();
				for (int k = i; k <= j; k++) {
					item.Add (k);
				}
				list.Add (item);
			} else if (sum > n) {
				break;
			}
		}
	}

	return list;
}

private static int Sum(int i,int j){
	int sum = 0;
	while (i <= j) {
		sum += i++;
	}
	return sum;
}
{% endhighlight %}

**解法2**

从另外一个角度出发，加入两个连续整数的和为n

那么可以得到的等式为：

x + (x+1) = n

2x = n - 1

如果是三个连续整数，那么很轻松可以得到等式：

3x = n - 1 - 2

以此类推

4x = n - 1 - 2 - 3

这样的话代码写起来就容易很多了：

{% highlight C# %} 
public static List<List<int>> GetNumList(int n){
	List<List<int>> list = new List<List<int>> ();
	var step = 1;
	var nums = 2;
	while ((n - step)>0) {
		if ((n - step) % nums == 0) {
			List<int> item = new List<int>();
			for (int i = 0; i < nums; i++) {
				item.Add ((n - step) / nums + i);
			}
			list.Add (item);
		}

		step += nums;
		nums++;
	}

	return list;
}
{% endhighlight %}