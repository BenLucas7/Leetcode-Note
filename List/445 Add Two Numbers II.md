# [445] Add Two Numbers II

|     Tag     |       Similar Problem        |  Difficulty   |
| :---------: | :--------------------------: | :-----------: |
| `traversal` | 2 | 🌟🌟 |

* You can find this problem here：[445. Add Two Numbers II](https://leetcode.com/problems/add-two-numbers-ii/)

* **Special thanks to Huahua 🥳** you can find the detailed solution of this problem in his blog：[花花酱 LeetCode 445. Add Two Numbers II](https://zxi.mytechroad.com/blog/list/leetcode-445-add-two-numbers-ii/)



### Problem definition：

You are given two **non-empty** linked lists representing two non-negative integers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.

**Follow up:**
What if you cannot modify the input lists? In other words, **reversing the lists is not allowed**.



### Example

```
Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 8 -> 0 -> 7
```



### Special cases:

1. 两个数字有不同的长度，e.g. `123 + 456789`
2. 相加之后数位变长，e.g. `11 + 99 = 110`



### Error Prone:

1. 因为是正序链表，涉及到数位对齐。
   1. 刚开始的想法是先遍历求得各链表长度，求出长度差`diff_len`，然后长的链表头指针后移`diff_len-1`，但该方法涉及到**同时需要前后两个指针**，因为存在后位进位的可能，实现比较复杂不是最优解。
   2. 可以用 `Stack`，将其转变成从最后一位开始相加的形式。
2. `Stack` 的实现可以用 python 内置的 `collection.deque` 双端队列实现。
   1. 创建一个栈 `d = deque()`
   2. 入栈 `d.append(x)`
   3. 获取栈顶元素并出栈 `d.pop()`
   4. 判断栈是否为空用 `len(d)`，没有 `d.empty()` 函数
3. **需要注意的是进位 `s` 应该存在于循环条件中**，当存在进位的时候还需要创建新的`ListNode`，对应special case-2。
4. 特别需要注意，`s//=10` 忽略会导致死循环



### Solution:

* Time complexity: $O(m+n)$
* Space complexity: $O(\max (m,n))$ 

```python
class Solution:
    def addTwoNumbers(self, l1: ListNode, l2: ListNode) -> ListNode:
        from collections import deque
        d1 = deque()
        d2 = deque()
        while l1:
            d1.append(l1.val)
            l1 = l1.next # 忘了会导致死循环
        while l2:
            d2.append(l2.val)
            l2 = l2.next # 忘了会导致死循环

        head = ListNode(0)
        s = 0
        while len(d1) or len(d2) or s:
            s+= (d1.pop() if d1 else 0) + (d2.pop() if d2 else 0) # 获取栈顶且弹出
            nxt = ListNode(s%10)
            nxt.next = head.next
            head.next = nxt
            s//=10

        return head.next
```



