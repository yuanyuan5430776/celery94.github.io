---
layout: post
title:  "Remove Nth Node From End of List - Leetcode No.19"
date:   2016-04-07
tags: 
    - algorithms
---

Given a linked list, remove the nth node from the end of list and return its head.

For example,

> Given linked list: 1->2->3->4->5, and n = 2.

> After removing the second node from the end, the linked list becomes 1->2->3->5.

**解答**

采用快慢指针的方案，当快指针移动到尾部的时候，慢指针就位于需要删除的节点处了。

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
    public ListNode RemoveNthFromEnd(ListNode head, int n) {
        var slow = head;
        var fast = head;
        while(n>0){
            fast = fast.next;
            n--;
        }
        
        if(fast == null){return slow.next;}

        while(fast.next !=null){
            slow = slow.next;
            fast = fast.next;
        }
        
        slow.next = slow.next.next;
        
        return head;
    }
}
{% endhighlight %}