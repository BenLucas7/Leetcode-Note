# [23] Merge k Sorted Lists

|     Tag     |       Similar Problem        |  Difficulty   |
| :---------: | :--------------------------: | :-----------: |
| `MergeSort` `PriorityQueue` | 21 | 🌟🌟🌟 |

* You can find this problem here：[23. Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/)

* **Special thanks to Hua hua** 🥳 you can find his solution (with video) here : [花花酱 LeetCode 23. Merge k Sorted Lists](https://zxi.mytechroad.com/blog/list/leetcode-23-merge-k-sorted-lists-2/)

  


### Problem definition：

Merge *k* sorted linked lists and return it as one sorted list. Analyze and describe its complexity.



### Example

```
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
```



### Idea

##### Priority Queue

1. Init an **PriorityQueue** using `from Queue import PriorityQueue `

2. Add each Linklist's **uncompared node** into the PriorityQueue 

3. Pop the `top`, if `top.next != None`, add `top.next` into the PriorityQueue
4. Loop until there's no element in the PriortyQueue
5. Assume there are $n$ elements in every sorted list and $k$ sorted lists in all
   1. Everytime when Priority Queue find the minimum in k elements, it will take $O(\log k)$ time.
   2. There are $n\cdot k$ elements in total, each needs to insert to and delete from the Priority Queue.
   3. So the time complexity is $O(nk\log k)$
   4. Space complexity is $O(k)$ if creating the result list not taking into account, otherwise, $O(k)+O(n)$

##### Merge Sort

1. Inspired by [21. Merge Two Sorted Lists](https://leetcode.com/problems/merge-two-sorted-lists/), we can use **divide and conquer** technique.
2. We can merge any two sorted lists, and merge the result with another sorted list and so on.
3. The time complexity is  $O(nk\log k)$, $\log k$ can stands for the height of the merge tree.
4. The space complexity is $O(\log k)$, for the reason that we implement merge sort in recursive way, and the space complexity has to do with the depth of the stack.



### Solution:

##### Priority Queue

* The time complexity is $O(nk\log k)$

* The space complexity is $O(k)$

* Accroding to official documentation

  > Tuple comparison breaks for (priority, task) pairs if the priorities are equal and the tasks do not have a default comparison order.

  The solution is  

  > A solution to the first two challenges is to store entries as **3-element list** including the priority, an entry count, and the task ... And since no two entry counts are the same, the tuple comparison will never attempt to directly compare two tasks.

```python
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
        import queue
        dummy = tail = ListNode(0)
        pq = queue.PriorityQueue()
        
        def add_node_in_pq(idx,node):
            if node:
                pq.put((node.val,idx,node))
        
        for idx,node in enumerate(lists): # idx stands for the kth list
            add_node_in_pq(idx,node)
        
        while not pq.empty():
            _,idx,node = pq.get()
            add_node_in_pq(idx,node.next)
            tail.next = node
            tail = tail.next
        
        return dummy.next
```



##### Merge Sort

* The time complexity is $O(nk\log k)$

* The space complexity is $O(\log k)$

```python
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:
    
        def merge(lists: List[ListNode], l:int, r:int) -> ListNode:
            if l>r: 
                return None
            elif l==r: # can not omit or line 13(14) could stuck in infinite loop
                return lists[l]
            elif l+1==r: # can be omitted but may cost more time
                return mergeTwoLists(lists[l],lists[r])
            else:
                m = (l+r)//2
                l1 = merge(lists,l,m)
                l2 = merge(lists,m+1,r)
                return mergeTwoLists(l1,l2)
        
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

            return dummy.next
        
        return merge(lists,0,len(lists)-1)
```



