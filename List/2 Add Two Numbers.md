# [2] Add Two Numbers

|     Tag     |          Similar Problem           |  Difficulty   |
| :---------: | :--------------------------------: | :-----------: |
| `traversal` | [445](./445 Add Two Numbers II.md) | $\star \star$ |

* You can find this problem here：[2. Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

* **Special thanks to Huahua 🥳** ，you can find the detailed solution of this problem (with video) in his blog：[花花酱 LeetCode 2. Add Two Numbers](https://zxi.mytechroad.com/blog/simulation/leetcode-2-add-two-numbers-2/)



### Problem definition：

You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.



### Special cases:

1. 两个数字有不同的长度，e.g. `123 + 456789`
2. 相加之后数位变长，e.g. `11 + 99 = 110`



### Error Prone:

1. 判断当前结点有效的方法是，`l1?=None`，if `True`，则当前结点有效，否则为空结点
2. 链表需要头尾指针，`dummy & tail`
3. 循环的条件是，两个链表指针与进位标志 `or`（不是`and`）操作，**因为只有一个存在有效结点，就需要并入到最终的返回链表中**。
4. 当有效结点和无效结点相加时，可以用判断语句，`l1.val if l1 else 0` 如果`l1`为空结点，则`l1.val=0`



### Solution:

```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummy = tail = ListNode(0)
        s = 0
        
        while l1 or l2 or s:
            s += (l1.val if l1 else 0) + (l2.val if l2 else 0)
            tail.next = ListNode(s % 10)
            tail = tail.next
            s //= 10
            l1 = l1.next if l1 else None
            l2 = l2.next if l2 else None
        
        return dummy.next
```



