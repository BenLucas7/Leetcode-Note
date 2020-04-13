# 141 Linked List Cycle

|     Tag     |       Similar Problem        |  Difficulty   |
| :---------: | :--------------------------: | :-----------: |
| `fast/slow` `Floyd’s Cycle Detection Algorithm` | 142 | 🌟🌟 |

* You can find this problem here：[141. Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/)

* **Special thanks to Huahua 🥳** you can find the detailed solution of this problem in his blog：[花花酱 LeetCode 141. Linked List Cycle](https://zxi.mytechroad.com/blog/list/leetcode-141-linked-list-cycle/)



### Problem definition：

Given a linked list, determine if it has a cycle in it.

To represent a cycle in the given linked list, we use an integer `pos` which represents the position (0-indexed) in the linked list where tail connects to. If `pos` is `-1`, then there is no cycle in the linked list.



### Example

```
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the second node.

3 -> 2 -> 0 -> 4
     |_________|
```



### Special cases:

1. 链表为空，e.g. `[]`
2. 链表只有一个结点，e.g. `[1]`



### Solution:

* Time complexity: $O(n)$
* Space complexity: $O(1)$

```python
class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        slow = fast = head
        while fast and fast.next: #避免单结点的 special case
            slow = slow.next 
            fast = fast.next.next 
            if slow == fast: # 判断要放在最后
                return True
        
        return False
```



### Floyd’s Cycle Detection Algorithm 

Floyd’s Cycle Detection Algorithm is a pointer algorithm that uses only two pointers, which move through the sequence **at different speeds**. 

The idea is to move `fast` pointer **twice as quickly as** the `slow` pointer and the distance between them increases by 1 at each step. If at some point both meet, we have found a cycle in the list, else if we have reached the end of the list, no cycle is present. 



### Reflection

1. 要注意 `while` 的循环条件 `while fast and fast.next`
   1. 如果是 `while fast` 则遇到单个结点的链表时，`fast = fast.next.next` 会报错，因为 `NoneType` 没有 `.next`
   2. 如果是 `while fast.next` 则遇到空链表时，进入 `while` 循环会报错。
   3. `fast.next == None` 表面已经走到了链表尽头，证明链表没有环。
2. 是非存在环路的判断条件 `if slow == fast` 应该放在两指针以后会判断，因为初始条件时，`slow = fast = head`，会直接返回 `True` 