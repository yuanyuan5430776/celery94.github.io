---
layout: post
title:  "Longest Palindromic Substring - Leetcode No.5"
date:   2016-03-10
categories: algorithms
---

**题目**

Given a string S, find the longest palindromic substring in S. 

You may assume that the maximum length of S is 1000, and there exists one unique longest palindromic substring.

**解答**

回文判断的要点是从中间字符挨个遍历左边与右边字符。

{% highlight C# %}
public string LongestPalindrome(string s) {
    int start =0,end =0;
    
    for(int i=0;i<s.Length;i++){
        int left =i, right = i;
        while(left>=0 && right<s.Length){
            if(s[left]==s[right]){
                if((right-left)>(end-start)){
                    start = left;
                    end = right;
                }
            }
            else{
                break;
            }
            left--;
            right ++;
        }
        
        left = i; right = i+1;
        while(left>=0 && right<s.Length){
            if(s[left]==s[right]){
                if((right-left)>(end-start)){
                    start = left;
                    end = right;
                }
            }
            else{
                break;
            }
            left--;
            right ++;
        }
    }
    
    var ret = "";
    for(int i=start;i<=end;i++){
        ret += s[i];
    }
    
    return ret;
}
{% endhighlight %}