---
layout: post
title:  "String to Integer (atoi) - Leetcode No.8"
date:   2016-03-22
tags: 
    - algorithms
---

Implement atoi to convert a string to an integer.

**解答**

最主要要考虑的问题就是溢出和正负问题。

{% highlight C# %}
public int MyAtoi(string str) {
    if(str == null || str.Length == 0){
        return 0;
    }
    
    int ret = 0 ;
    int i = 0;
    bool flag = false;
    while(str[i]==' ' && i < str.Length){
        i++;
    }
    
    if(str[i] == '-' || str[i] == '+'){
        
        if(str[i] == '-'){
            flag = true;
        }
        
        i++;
    }
    
    for(;i<str.Length;i++){
        int c = (int)str[i] - 48;
        if(c<0 || c>9){
            return  (flag && ret!=int.MinValue) ? 0 - ret : ret;
        }
        else{
            
            if(flag){
                if((int.MinValue + c) /10 > 0 - ret){
                    return int.MinValue;
                }
            }
            else{
                if((int.MaxValue - c) / 10 < ret){
                    return int.MaxValue;
                }    
            }
            
            ret = ret * 10 + c;
        }
    }
    
    return  flag?0-ret:ret;
}
{% endhighlight %}