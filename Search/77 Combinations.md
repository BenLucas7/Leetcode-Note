# 77 Combinations

|         Tag         |     Similar Problem     | Difficulty |
| :-----------------: | :---------------------: | :--------: |
| `combination` `DFS` | 17, 39, 40, 78, 90, 216 |     🌟🌟     |

* You can find this problem here：[77. Combinations](https://leetcode.com/problems/combinations/)



### Problem definition：

Given two integers *n* and *k*, return all possible combinations of *k* numbers out of 1 ... *n*.



### Example:

```
Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```



### Methodology:

* It is a **combination** issue.

* Use `vector` to save the answer, instead of return an element.

* **In order not to scan from the beginning everytime**, we can **sort first** and pass `begin position` as a parameter to the DFS.

* Use `depth` to control the execution of DFS function.

* There should be a push and pop before and after the DFS function.

  ```c++
  for(auto elem:candidates){
      cur.push_back(elem) # start from elem
      DFS() # find the solution begin with elem
      cur.pop_back() # all the solution begin with elem has been considered, forget it.
  }
  ```

  

### Tamplate of Combinations:

```cpp
void DFS(vector<int>&nums,int depth,int n,int position,
         vector<int>&curr,vector<vector<int>>&ans){
    if(depth == n){
	ans.push_back(curr);
	return;
    }
}

for(int i=position; i<nums.size();i++){
    curr.push_back(nums[i]);
    DFS(nums,depth,n,position+1,curr,ans) // position+1 avoids duplication.
    curr.pop_back();
}
```



### Solution:

```c++
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> ans;
        vector<int> cur;
        vector<int> candidates;
        for(int i=1;i<=n;i++){
            candidates.push_back(i);
        }
                
        dfs(candidates, k, 0, cur, ans);
        
        return ans;
    }
    
private:
    void dfs(vector<int>& candidates, int depth, int posi, 
             vector<int>& cur, vector<vector<int>>& ans){
        if(depth==0){
            ans.push_back(cur);
            return;
        }
        
        for(int i=posi;i<candidates.size();i++){
            cur.push_back(candidates[i]);
            dfs(candidates,depth-1, i+1, cur, ans);
            cur.pop_back();
        }
    }
};
```



