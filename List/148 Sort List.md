# 148 Sort List

|     Tag     |       Similar Problem        |  Difficulty   |
| :---------: | :--------------------------: | :-----------: |
| `bottom up` `merge sort` |  | 🌟🌟🌟🌟 |

* You can find this problem here：[148. Sort List](https://leetcode.com/problems/sort-list/)

* **Special thanks to Huahua 🥳** you can find the detailed solution with video of this problem in his blog: [花花酱 LeetCode 148. Sort List](https://zxi.mytechroad.com/blog/divide-and-conquer/leetcode-148-sort-list/)

### Problem definition：

Sort a linked list in $O(n \log n)$  time using **constant space complexity**.



### Example

```
Input: 4->2->1->3
Output: 1->2->3->4
```



### Idea

* **Special thanks to ChrisTrompf**  🥳 you can find his tons of detailed explanation [here](https://leetcode.com/problems/sort-list/discuss/166324/c%2B%2B-and-java-legit-solution.-O(nlogn)-time-and-O(1)-space!-No-recursion!-With-detailed-explaination)

* The problem calls for $O(1)$ space, **so we can not use recursion**. Quicksort is out, merge sort is in.

* The idea is to **merge progressively larger sublists together** until the resulting list is sorted. 

* Use `[3, 45, 2, 15, 37, 19]` for instance

  | Progress               | Step size | Sublists                         | Merged                     |
  | ---------------------- | --------- | -------------------------------- | -------------------------- |
  | [3, 45, 2, 15, 37, 19] | 1         | [3], [45], [2], [15], [37], [19] | [3, 45], [2, 15], [19, 37] |
  | [3, 45, 2, 15, 19, 37] | 2         | [3, 45], [2, 15], [19, 37]       | [2, 3, 15, 45], [19, 37]   |
  | [2, 3, 15, 45, 19, 37] | 4         | [2, 3, 15, 45], [19, 37]         | [2, 3, 15, 19, 37, 45]     |

  1. Grab two sorted lists of size `step`
  2. Merge the two lists into a single sorted list of size `step*2` and reattach to input list
  3. Repeat from step (1) until entire list has been exhausted



### Solution:

* The time complexity is $O(n\log n)$
* The space complexity is $O(1)$

```python
class Solution:
    def sortList(self, head: ListNode) -> ListNode:
        
        # Splits the list into two parts, first n element and the rest.
  	# Returns the head of the rest.
        def split(head: ListNode, n:int) -> ListNode:
            while n>1 and head:
                head = head.next
                n-=1
            rest = head.next if head else None
            if head: 
                head.next = None
            return rest
             

	# Merges two lists, returns the head and tail of the merged list.
        def mergeTwoLists(l1: ListNode, l2: ListNode) -> ListNode:
            dummy = tail = ListNode(0)

            while l1 and l2:
                if l1.val <= l2.val:
                    tail.next = l1
                    l1 = l1.next
                else:
                    tail.next = l2
                    l2 = l2.next
                tail = tail.next 

            tail.next = l1 or l2 
            while(tail.next):
              	tail = tail.next

            return dummy.next,tail
        
        # 0 or 1 element, we are done.
        if not head or not head.next: return head
        
        length = 1
        cur = head
        while cur:
            length+=1
            cur = cur.next
        
        dummy = ListNode(0)
        dummy.next = head
        
        n = 1
        while n<length:
            cur = dummy.next
            tail = dummy
            while cur: # progressively merge two n length list
                l = cur
                r = split(l,n)
                cur = split(r,n)
                subMerged,subTail = mergeTwoLists(l,r) 
                tail.next = subMerged
                tail = subTail
            n*=2
        
        return dummy.next
          
```

* Use `[3, 45, 2, 15, 37, 19]` for instance, line 51 to 55 could be explain as followed
  * `n = 1`
    * `dummy:[]`,  `l:[3]`, `r:[45]`, `cur:[2,15,37,19]`
    * `dummy:[3,45]`, `l:[2]`, `r:[15]`, `cur:[37,19]`
    * `dummy:[3,45,2,15]`, `l:[37]`, `r:[19]`, `cur:[]`
    * `dummy:[3,45,2,15,19,37]`, `l:[]`, `r:[]`, `cur:[]`
  * `n = 2`
    * `dummy:[]`,  `l:[3,45]`, `r:[2,15]`, `cur:[19,37]`
    * `dummy:[2,3,15,45]`, `l:[19,37]`, `r:[]`, `cur:[]`
    * `dummy:[2,3,15,45,19,37]`, `l:[]`, `r:[]`, `cur:[]`
  * `n = 4`
    * `dummy:[]`,  `l:[2,3,15,45]`, `r:[19,37]`, `cur:[]`
    * `dummy:[2,3,15,19,37,45]`, `l:[]`, `r:[]`, `cur:[]`
  * Finished.

* **This solution enlightened me that we can split the list with given length progressively (w.r.t `l`,`r`,`cur` ).** 