# 39 Combination Sum

|         Tag         |     Similar Problem     | Difficulty |
| :-----------------: | :---------------------: | :--------: |
| `combination` `DFS` | 17, 40, 77, 78, 90, 216 |     🌟🌟     |

* You can find this problem here：[39. Combination Sum](https://leetcode.com/problems/combination-sum/)
* **Special thanks to Huahua** 🥳 you can find the detailed solution of this problem (with video) in his blog：[花花酱 LeetCode 39. Combination Sum](https://zxi.mytechroad.com/blog/searching/leetcode-39-combination-sum/)



### Problem definition：

Given a **set** of candidate numbers (`candidates`) **(without duplicates)** and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

The **same** repeated number may be chosen from `candidates` unlimited number of times.

**Note:**

- All numbers (including `target`) will be positive integers.
- The solution set must not contain duplicate combinations.



### Example:

```
Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
```



### Methodology:

* It is a **combination** issue.

* Use `vector` to save the answer, instead of return an element.

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
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> ans;
        vector<int> cur;
        sort(candidates.begin(), candidates.end());
        dfs(candidates, target, 0, cur, ans);
        
        return ans;
    }
    
private:
    void dfs(vector<int>& candidates, int target, int posi, 
             vector<int>& cur, vector<vector<int>>& ans){
        if(target==0){
            ans.push_back(cur);
            return; // write return; statement to indicate end of function 
        }
        
        for(int i=posi;i<candidates.size();i++){
            if(candidates[i]>target)
                break;
            cur.push_back(candidates[i]);
            dfs(candidates, target-candidates[i], i, cur, ans);
            cur.pop_back();
        }
        
    }
    
};
```



