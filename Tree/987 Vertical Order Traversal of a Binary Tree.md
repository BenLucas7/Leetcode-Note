# 987 Vertical Order Traversal of a Binary Tree

|     Tag     |       Similar Problem        |  Difficulty   |
| :---------: | :--------------------------: | :-----------: |
| `tree traversel` | 1302 | 🌟 |

* You can find this problem here：[987. Vertical Order Traversal of a Binary Tree](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/)




### Problem definition：

Given a binary tree, return the *vertical order* traversal of its nodes values.

For each node at position `(X, Y)`, its left and right children respectively will be at positions `(X-1, Y-1)` and `(X+1, Y-1)`.

Running a vertical line from `X = -infinity` to `X = +infinity`, whenever the vertical line touches some nodes, we report the values of the nodes in order from top to bottom (decreasing `Y` coordinates).

If two nodes have the same position, then the value of the node that is reported first is the value that is smaller.

Return an list of non-empty reports in order of `X` coordinate. Every report will have a list of values of nodes.



### Example

<img src="./pic/987.png" style="zoom:38%;" />

```
Input: [1,2,3,4,5,6,7]
Output: [[4],[2],[1,5,6],[3],[7]]
Explanation: 
The node with value 5 and the node with value 6 have the same position according to the given scheme.
However, in the report "[1,5,6]", the node value of 5 comes first since 5 is smaller than 6.
```



### Methodology:

* Tag every node with coordinations (x,y).
* **In the same location (x,y), node should be stored in ascent order, we can use `set` since internally, the elements in a `set` are always sorted.** 
* Use `map<int,map<int,set<int>>>` to store the answer.
* Traverse the tree in preorder.



### Solution:

* Time complexity: $O(n)$
* Space complexity: $O(n)$

```cpp
class Solution {
public:
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        map<int,map<int,set<int>>> m;
        vector<vector<int>> ans;
        Traversal(root,0,0,m);
        
        for(auto itx=m.begin();itx!=m.end();itx++){
            ans.push_back(vector<int>());
            for(auto ity=itx->second.begin();
               ity!=itx->second.end();ity++){
                ans.back().insert(end(ans.back()),begin(ity->second),end(ity->second));
            }
        }
        
        return ans;
    }

private:
    void Traversal(TreeNode* root,int x,int y, map<int,map<int,set<int>>>& m){
        if(!root){
            return;
        }
        
        m[x][y].insert(root->val);
        Traversal(root->left,x-1,y+1,m); // if use y-1 as the definition, 
        Traversal(root->right,x+1,y+1,m);// the order in the vector would be descent
    }
};
```


