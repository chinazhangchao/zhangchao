---
title: "LeetCode-2. Add Two Numbers（两数相加）C++实现"
date: 2019-08-24T11:54:08+08:00
type: posts
categories: [LeetCode,C++]
keywords: [LeetCode,算法,C++]
tags: [LeetCode,C++]
---

给出两个 **非空** 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 **逆序** 的方式存储的，并且它们的每个节点只能存储 **一位** 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。
<!--more-->
**示例：**
```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```
下面给出C++代码的解法
```cpp
/**
* Definition for singly-linked list.
* struct ListNode {
*     int val;
*     ListNode *next;
*     ListNode(int x) : val(x), next(NULL) {}
* };
*/
class Solution {
public:
	ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
		// 注意运用dummy技巧很多时候可以让代码更简洁
		ListNode dummy(0);
		ListNode* prev = &dummy;
		int carry = 0;

		while (l1 != nullptr || l2 != nullptr || carry != 0) {
			int sum = (l1 == nullptr ? 0 : l1->val) + (l2 == nullptr ? 0 : l2->val) + carry;
			carry = sum / 10;
			prev->next = new ListNode(sum % 10);
			prev = prev->next;
			l1 = (l1 == nullptr ? nullptr : l1->next);
			l2 = (l2 == nullptr ? nullptr : l2->next);
		}

		return dummy.next;
	}
};
```
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        prev = root = ListNode(0)
        carry = 0
        
        while l1 or l2 or carry:
            sum = (l1.val if l1 else 0) + (l2.val if l2 else 0) + carry
            carry = sum // 10
            prev.next = ListNode(sum % 10)
            prev = prev.next
            l1 = l1.next if l1 else None
            l2 = l2.next if l2 else None
        
        return root.next
```
**复杂度分析**

- 时间复杂度：O(max(m, n))，假设 m 和 n 分别表示 l1 和 l2 的长度，上面的算法最多重复 max(m, n) 次。

- 空间复杂度：O(max(m, n))，新列表的长度最多为 max(m,n) + 1。

**原题地址**
英文版：https://leetcode.com/problems/add-two-numbers/
中文版：https://leetcode-cn.com/problems/add-two-numbers/
