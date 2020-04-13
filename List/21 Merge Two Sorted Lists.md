# 21 Merge Two Sorted Lists

|     Tag     |       Similar Problem        |  Difficulty   |
| :---------: | :--------------------------: | :-----------: |
| `merge sort` | 23 | 🌟 |

* You can find this problem here：[21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/)

  


### Problem definition：

Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.



### Example

```
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
```



### Idea

* use two pointer to scan the lists
* compare the value and add the less one to the merged list
* add the remaining list of `l1` or `l2` to the merged list



### Solution:

* The time complexity is $O(m+n)$
* The space complexity is $O(1)$

```python
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummy = tail = ListNode(0)
        
        while l1 and l2:
            if l1.val <= l2.val:
                tail.next = l1
                l1 = l1.next
            else:
                tail.next = l2
                l2 = l2.next
            tail = tail.next # do not forget to move tail
        
        tail.next = l1 or l2 # add the remining list, l1 if l1 else l2 costs more time
        
        return dummy.next
```





