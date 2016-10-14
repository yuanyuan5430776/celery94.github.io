---
layout: post
title:  "Merge Two Sorted Lists - Leetcode No.21"
date:   2016-04-13
tags: 
    - algorithms
---

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.

**解答**

采用递归的方式来解决问题。

{% highlight C# %}
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     public int val;
 *     public ListNode next;
 *     public ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public ListNode MergeTwoLists(ListNode l1, ListNode l2) {
        if(l1 == null && l2 == null) return null;
        
        if(l1 == null) return l2;
        if(l2 == null) return l1;
        
        if(l1.val < l2.val){
            l1.next = MergeTwoLists(l1.next,l2);
            return l1;
        }
        else{
            l2.next = MergeTwoLists(l2.next,l1);
            return l2;
        }
    }
}
{% endhighlight %}