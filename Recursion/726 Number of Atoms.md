# 726 Number of Atoms

|     Tag     | Similar Problem | Difficulty |
| :---------: | :-------------: | :--------: |
| `traversal` |       445       |     🌟🌟     |

* You can find this problem here：[726. Number of Atoms](https://leetcode.com/problems/number-of-atoms/)
* `recursion` **Special thanks to Huahua** 🥳 you can find the detailed solution of this problem (with video) in his blog：[花花酱 LeetCode 726. Number of Atoms](https://zxi.mytechroad.com/blog/string/leetcode-726-number-of-atoms/)
* `stack` **Special thanks to yangshun** 🥳 you can find the detailed solution of this problem in his leetcode answer：[Neat Python with Explanation - 35ms](https://leetcode.com/problems/number-of-atoms/discuss/109333/Neat-Python-with-Explanation-35ms)



### Problem definition：

Given a chemical `formula` (given as a string), return the count of each atom.

An atomic element always starts with an uppercase character, then zero or more lowercase letters, representing the name.

1 or more digits representing the count of that element may follow if the count is greater than 1. If the count is 1, no digits will follow. For example, H2O and H2O2 are possible, but H1O2 is impossible.

Two formulas concatenated together produce another formula. For example, H2O2He3Mg4 is also a formula.

A formula placed in parentheses, and a count (optionally added) is also a formula. For example, (H2O2) and (H2O2)3 are formulas.

Given a formula, output the count of all elements as a string in the following form: the first name (in sorted order), followed by its count (if that count is more than 1), followed by the second name (in sorted order), followed by its count (if that count is more than 1), and so on.



### Example:

```
Input: formula = "K4(ON(SO3)2)2"
Output: "K4N2O14S4"
Explanation: The count of elements are {'K': 4, 'N': 2, 'O': 14, 'S': 4}.
```



### Methodology:

#### Recursion

#### Stack

* Use regex `([A-Z]{1}[a-z]?|\(|\)|\d+)` to split given string into required tokens

  * An input of `Mg(OH)2` will be tokenized into: `['Mg', '(', 'O', 'H', ')', '2']`.
  * An input of `K4(ON(SO3)2)2` will be tokenized into: `['K', '4', '(', 'O', 'N', '(', 'S', 'O', '3', ')', '2', ')', '2']`.

* For every atom, we use a dict to store itself and its count, like `{Mg:1}`

* Initilaize a stack with a dict

  * if `token` is a atom name, `count` equals to 1 or following number, add it into former dict
  * if `token == '('` , push another dict into the stack
  * if `token == ')'` , pop the top of stack, multiply with count and merge it into former dict
  * **Notice, all the elements in stack is a dict**, so there can always be a former dict to merge into.

* Use `Mg(OH)2` for instance

  ```python
  [defaultdict(<type 'int'>, {'Mg': 1})]
  [defaultdict(<type 'int'>, {'Mg': 1}), defaultdict(<type 'int'>, {})]
  [defaultdict(<type 'int'>, {'Mg': 1}), defaultdict(<type 'int'>, {'O': 1})]
  [defaultdict(<type 'int'>, {'Mg': 1}), defaultdict(<type 'int'>, {'H': 1, 'O': 1})]
  [defaultdict(<type 'int'>, {'H': 2, 'Mg': 1, 'O': 2})]
  H2MgO2
  ```

  

### Solution:

#### Stack

* Time complexity: $O(n)$
* Space conplexity: $O(1)$

```python
class Solution:
    def countOfAtoms(self, formula):
        import re
        from collections import defaultdict
        tokens = list(filter(lambda c: c, re.split('([A-Z]{1}[a-z]?|\(|\)|\d+)', formula))) # split the formula
        stack, i = [defaultdict(int)], 0 # init stack with a dict
        while i < len(tokens):
            token = tokens[i]
            if token == '(':
                stack.append(defaultdict(int))
            else:
                count = 1
                # Check if next token is a number.
                if i + 1 < len(tokens) and tokens[i + 1].isdigit():
                    count, i = int(tokens[i + 1]), i + 1
                if token == ')':
                    atoms = stack.pop()
                else:
                    atoms = { token: 1 }
                # Combine counts of atoms to former dict.
                for atom in atoms:
                    stack[-1][atom] += atoms[atom] * count
            i += 1
        return ''.join([atom + (str(count) if count > 1 else '') for atom, count in sorted(stack[-1].items())])

```



