# 1302 Deepest Leaves Sum

|     Tag     |       Similar Problem        |  Difficulty   |
| :---------: | :--------------------------: | :-----------: |
| `level order traversal` | 94, 144, 145, 429, 589, 590, 987 | 🌟 |

* You can find this problem here：[1302. Deepest Leaves Sum](https://leetcode.com/problems/deepest-leaves-sum/)




### Problem definition：

Given a binary tree, return the sum of values of its deepest leaves.



### Example

![](./pic/1302.png)

```
Input: root = [1,2,3,4,5,null,6,7,null,null,null,null,8]
Output: 15
```



### Methodology:

* Use **level order traversal** to get the sum of last level, for more information about level order traversal, you can read *429 N-ary Tree Level Order Traversal*

### Solution:

* Time complexity: $O(n)$
* Space complexity: $O(n)$

```cpp
class Solution {
public:
    int deepestLeavesSum(TreeNode* root) {
        queue<TreeNode*> q;
        
        q.push(root);
        int sum = root->val;
        while(!q.empty()){
            int n = q.size();
            sum = 0;
            for(int i=0;i<n;i++){
                TreeNode* f = q.front();
                sum += f->val;
                q.pop();
                if(f->left) q.push(f->left);
                if(f->right) q.push(f->right);
            }
        }
        
        return sum;
    }  
};
```
