# 40 Combination Sum II

|         Tag         |     Similar Problem     | Difficulty |
| :-----------------: | :---------------------: | :--------: |
| `combination` `DFS` | 17, 39, 77, 78, 90, 216 |     🌟🌟     |

* You can find this problem here：[40. Combination Sum II](https://leetcode.com/problems/combination-sum-ii)



### Problem definition：

Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

Each number in `candidates` may only be used **once** in the combination.

**Note:**

- All numbers (including `target`) will be positive integers.
- The solution set must not contain duplicate combinations.



### Example:

```
Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
```



### Methodology:

* It is a **combination** issue.

* Use `vector` to save the answer, instead of return an element.

* **Use `set` to save all the vector combination answer in order to avoid duplication.** (different from problems 39)

* **In order not to scan from the beginning everytime**, we can **sort first** and pass `begin position` as a parameter to the DFS.

* After sorted, when we select an element `elem`

  * if `elem > target`, prune it cause there are no small elements left behind.
  * if `elem == target`, we find a solution! save it and `return;`
  * if `elem < target`, call `DFS` with start at the same position and with `target-elem` as the new target.

* There should be a push and pop before and after the DFS function

  ```c++
  for(auto elem:candidates){
  		cur.push_back(elem) # start from elem
  		DFS() # find the solution begin with elem
  		cur.pop_back() # all the solution begin with elem has been considered, forget it.
  }
  ```

  

### Solution:

```c++
class Solution {
public:
    vector<vector<int>> combinationSum2(vector<int>& candidates, int target) {
        set<vector<int>> ans; // avoid duplication
        vector<int> cur;
        sort(candidates.begin(), candidates.end());
        dfs(candidates, target, 0, cur, ans);
        
        return vector<vector<int>>(ans.begin(),ans.end());
    }
    
private:
    void dfs(vector<int>& candidates, int target, int posi, 
             vector<int>& cur, set<vector<int>>& ans){
        if(target==0){
            ans.insert(cur);
            return;
        }
        
        for(int i=posi;i<candidates.size();i++){
            if(candidates[i]>target)
                break;
            
            cur.push_back(candidates[i]);
            dfs(candidates, target-candidates[i], i+1, cur, ans);
            cur.pop_back();
        }
    }
};
```



