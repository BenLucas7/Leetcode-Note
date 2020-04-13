# 100 Same Tree

|     Tag     |       Similar Problem        |  Difficulty   |
| :---------: | :--------------------------: | :-----------: |
| `traversal` |  | 🌟 |

* You can find this problem here：[100. Same Tree](https://leetcode.com/problems/same-tree/)




### Problem definition：

Given two binary trees, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical and the nodes have the same value.



### Example

```
Input:     1         1
          / \       / \
         2   3     2   3

        [1,2,3],   [1,2,3]

Output: true

Input:     1         1
          /           \
         2             2

        [1,2],     [1,null,2]

Output: false
```



### Methodology:

* There are only three cases as below
  * `p` and `q` is not `NULL`, then judge the value.
  * either `p` or `q` is `NULL`, return `false`
  * both `p` and `q` are `NULL`, return `true`

### Solution:

* Time complexity: $O(n)$
* Space complexity: $O(1)$

```cpp
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if(!p||!q) return p==q; // if one is NULL
        return p->val==q->val && isSameTree(p->left,q->left) && isSameTree(p->right,q->right);
    }
};
```


