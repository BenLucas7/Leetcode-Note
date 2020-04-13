# 24 Swap Nodes in Pairs

|     Tag     |       Similar Problem        |  Difficulty   |
| :---------: | :--------------------------: | :-----------: |
| `reverse` |  | 🌟🌟 |

* You can find this problem here：[24. Swap Nodes in Pairs](https://leetcode.com/problems/swap-nodes-in-pairs/)

* **Special thanks to Huahua 🥳** you can find the detailed solution of this problem in his blog：[花花酱 LeetCode 24. Swap Nodes in Pairs](https://zxi.mytechroad.com/blog/list/leetcode-24-swap-nodes-in-pairs/)



### Problem definition：

Given a linked list, swap every two adjacent nodes and return its head.

You may **not** modify the values in the list's nodes, only nodes itself may be changed.



### Example

```
Input: 1 -> 2 -> 3 -> 4
Output: 2 -> 1 -> 4 -> 3
```



### Special cases:

1. 链表长度为奇数，e.g. `1`， `1 -> 2 -> 3`



### Solution:

* Time complexity: $O(n)$
* Space complexity: $O(1)$

```python
class Solution:
    def swapPairs(self, head: ListNode) -> ListNode:
        dummy = ListNode(0)
        dummy.next = head
        head = dummy
        while head.next and head.next.next:
            n1,n2 = head.next, head.next.next
            n1.next = n2.next
            n2.next = n1
            head.next = n2
            head = n1
        
        return dummy.next
```



### My Mistake

```python
while p and p.next:
    tmp = p.next.next # 记录下一个待交换，如 1 -> 2 -> 3 -> 4 中的 3
    p.next.next = p # 2 指向 1
    p.next = tmp # 1 指向 3
    p = tmp # 从 3 处重新循环
```

这个第一轮看似正确，但是错误的原因在于，后续操作没有将链表片段连接起来，比如

```
Input：1 -> 2 -> 3 -> 4
Output：1 -> 3 (原因如下)

         4 -> 3 -> None
              ↑
         2 -> 1
              ↑
             head
```

1. 虽然 `1` 和 `2` 换位了，但是 `head` 一直指着 `1`
2. `3` 和 `4` 交换位置后，`1` 并没有和 `4` 相连，所以最终输出的只是链表的片段
3. **所以，需要一个交换位置之前的指针连接交换后的链表片段**
   1. 从 `pre -> a -> b -> b.next` 到 `pre -> b -> a -> b.next` 的形式，需要更改三处指针，即 `pre.next, b.next, a.next = b, a, b.next`



### Reflection

1. `while head.next and head.next.next:` 
   1. `while` 的判断条件同时考虑 `.next` 与 `.next.next`，即对单个剩余结点不采取操作，针对 special case
   2. 如果固定 `head` 指针不变，即 `head` 在 `while` 循环之外，则交换打头两个结点需要更改一次 `head.next` ，否则会丢失链表头结点，且需要一个遍历指针 `p` 对后续链表进行遍历交换，显得繁琐。
   3. 如果不固定 `head` 指针，即 `head` 在 `while` 循环之内，则 `head` 可以充当遍历指针 `p` ，不需要在循环外独立操作
2. **`head` 的作用是接上交换后的链表**，如果交换指针指向的是**当前需要**交换的结点，就不能接上之前的链表（如 My Mistake），所以需要一个前向指针，即 `head`