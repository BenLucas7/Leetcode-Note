# [147] Insertion Sort List

|     Tag     |       Similar Problem        |  Difficulty   |
| :---------: | :--------------------------: | :-----------: |
| `insertion sort` |  | 🌟🌟🌟 |

* You can find this problem here：[147. Insertion Sort List](https://leetcode.com/problems/insertion-sort-list/)

* **Special thanks to GraceMeng** 🥳  you can find his solution here: [GraceMeng](https://leetcode.com/problems/insertion-sort-list/discuss/190913/Java-Python-with-Explanations)

  

### Problem definition：

Sort a linked list using insertion sort.

![img](pic/147.gif)

A graphical example of insertion sort. The partial sorted list (black) initially contains only the first element in the list.
With each iteration one element (red) is removed from the input data and inserted in-place into the sorted list

**Algorithm of Insertion Sort:**

1. Insertion sort iterates, consuming one input element each repetition, and growing a sorted output list.
2. At each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there.
3. It repeats until no input elements remain.



### Example

```
Input: 4->2->1->3
Output: 1->2->3->4
```



### Idea

```
dummy0 -> 1 -> 3 -> 5 -> 2 -> 4 - > null
          |         |    |
      toInsertPre  scan  toInsert

-- locate toInsert = 2 by scan.val > scan.next.val
-- locate toInsertPre = 1 by toInsertPre.next.val > toInsert.val
-- insert toInsert between toInsertPre and toInsertPre.next
```

* ⚠️ **Attention**:  `toInsertPre` may be **far away** from `toInset`, so using  `toInsetPre.next.next = toInsert.next` to catch the linklist after `toInsert` can go wrong.



### Solution:

* The time complexity is $O(n^2)$
* The space complexity is $O(1)$

```python
class Solution:
    def insertionSortList(self, head):

        dummyHead = ListNode(0)
        dummyHead.next = scan = head
        
        while scan and scan.next: # consider Linklist with more than 2 nodes
            if scan.val > scan.next.val:
                # Locate nodeToInsert.
                nodeToInsert = scan.next
                
                # Locate nodeToInsertPre.
                nodeToInsertPre = dummyHead
                while nodeToInsertPre.next.val < nodeToInsert.val:
                    nodeToInsertPre = nodeToInsertPre.next
                    
                scan.next = nodeToInsert.next
                # Insert nodeToInsert between nodeToInsertPre and nodeToInsertPre.next.
                nodeToInsert.next = nodeToInsertPre.next
                nodeToInsertPre.next = nodeToInsert
            else: # scan a descent trend
                scan = scan.next
            
        return dummyHead.next
```



### Beyond

Although this problem asks to use insertion sort in advance, let's take it a little bit further. 

According to [Steve Yegge's tips on interviewing at Google](http://steve-yegge.blogspot.com/2008/03/get-that-job-at-google.html), *"For God's sake, don't try sorting a linked list during the interview."* It may actually be faster to copy the list to an array and then use a [Quicksort](http://en.wikipedia.org/wiki/Quicksort). So the solution could be rewritten as below. 

```python
class Solution:
    def insertionSortList(self, head: ListNode) -> ListNode:
        
        array = []
        scan = head
        while scan:
            array.append(scan.val)
            scan = scan.next
        array.sort()
    
        idx = 0
        scan = head
        while scan:
            scan.val = array[idx]
            scan = scan.next
            idx += 1
        
        return head
```

* since we need to scan twice, one for forming an array, the other for copying it back, so the time complexity is $O(2n+n\log n) = O(n\log n)$, the space complexity is $O(n)$

* According to the [StackOverflow answer](https://stackoverflow.com/questions/1525117/whats-the-fastest-algorithm-for-sorting-a-linked-list/1525419#1525419), The reason this might be faster is that **an array has much better cache performance than a linked list**. If the nodes in the list are dispersed in memory, you may be generating cache misses all over the place. Then again, if the array is large you will get cache misses anyway.

* **Mergesort parallelises better**, so it may be a better choice if that is what you want. It is also much faster if you perform it directly on the linked list.

* Here is an comparison of the Linklist sorting rate

  > N = 100000000:
  >
  > Fragmented list with merge sort: 364.797000 seconds
  >
  > Array with qsort: 61.166000 seconds
  >
  > Packed list with merge sort: 16.525000 seconds

