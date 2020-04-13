# 429 N-ary Tree Level Order Traversal

|            Tag            | Similar Problem | Difficulty |
| :-----------------------: | :-------------: | :--------: |
| `level order` `traversal` |  102, 107, 872  |     🌟🌟     |

* You can find this problem here：[429. N-ary Tree Level Order Traversal](https://leetcode.com/problems/n-ary-tree-level-order-traversal/)



### Problem definition：

Given an n-ary tree, return the *level order* traversal of its nodes' values.

*Nary-Tree input serialization is represented in their level order traversal, each group of children is separated by the null value* 

<img src="./pic/429.png" alt="429" style="zoom:50%;" />

### Example:

```
Input: root = [1,null,3,2,4,null,5,6]
Output: [[1],[3,2,4],[5,6]]
```



### Methodology:

* Use a **queue**, when the pop the front, push its children back into the queue.

* How to judge whether a level is finished traversing or not?

  * When traverse every level, we push the root first, so we can **record the size of the queue** which indicates how many nodes in this level.
  * For instance, we push `root` first, and the size of queue is 1, which indicates this level only contains 1 node. So we only get the front of the queue 1 times, after that, the traverse of this level finishes.
  * When we get the front of the queue, which is `root` in this example, we push its children into the queue, which are `2,3,4`. 
  * After we pop `root`, the size of the queue is 3, which means this level has 3 nodes and we need to get the front of the queue 3 times.
  * Repeat above.

  

### Solution:

* Time complexity: $O(n)$
* Space complexity: $O(n)$ 

```c++
class Solution {
public:
    vector<vector<int>> levelOrder(Node* root) {
        queue<Node*> q;
        vector<vector<int>> ans;
        
        if(!root) 
            return ans;
        
        q.push(root);
        while(!q.empty()){
            int n = q.size(); // record the nums of node of this level
            vector<int> cur;
            for(int i=0;i<n;i++){
                Node* f = q.front();
                cur.push_back(f->val);
                q.pop();
                for(int j=0;j<f->children.size();j++) // vector<Node*> children
                    q.push(f->children[j]);
            }
            ans.push_back(cur);
        }
        return ans;
    }
};
```



