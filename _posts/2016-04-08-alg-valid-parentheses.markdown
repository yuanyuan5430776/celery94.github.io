---
layout: post
title:  "Valid Parentheses - Leetcode No.20"
date:   2016-04-08
categories: algorithms
---

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

**解答**

采用栈的方式来实现判断就非常的简单了。

{% highlight C# %}
public class Solution {
    public bool IsValid(string s) {
        Stack<char> stack = new Stack<char>();
        
        foreach(var c in s){
            if(c=='(' || c=='{' || c=='['){
                stack.Push(c);
            }
            
            if(c == ')' && (stack.Count==0 || stack.Pop()!='(')){
                return false;
            }
            if(c == '}' && (stack.Count==0||stack.Pop()!='{')){
                return false;
            }
            if(c == ']' && (stack.Count==0||stack.Pop()!='[')){
                return false;
            }
        }
        
        return stack.Count==0;
    }
}
{% endhighlight %}