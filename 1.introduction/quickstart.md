# Quick Start

## Data Structure and Algorithm

Data structure is a form of data, such as linked list, binary tree, stack, queue, etc. are all forms of data representaion in memory. Algorithm is a general problem-solving template or idea, and most data structure utilize a set of general algorithm templates.  
Therefore, you can solve various algorithmic problems by mastering these general templates.  

First introduce two algorithm problems, try to feel ~

**Example 1**

- [x] [strStr](https://leetcode-cn.com/problems/implement-strstr/)

> Return the index of the first occurrence of `needle` in `haystack`, or -1 if `needle` is not part of `haystack`.

```c++
int strStr(string haystack, string needle) {
    int len_h = haystack.length(), len_n = needle.length();
    if (len_n == 0) {
        return 0;
    }
    int i = 0, j = 0;
    for ( ; i + len_n <= len_h; i++) {
        j = 0;
        for ( ; j < len_n; j++) {
            if (haystack[i + j] != needle[j]) {
                break;
            }
        }
        if (j == len_n) {
            return i;
        }
    }
    return -1;
}
```

**Example 2**

- [ ] [subsets](https://leetcode-cn.com/problems/subsets/)

> Given an integer array `nums` of unique elements, return all possible subsets (the power set).

*Method 1: Enumerate subsets by masks.*

```c++
class Solution {
public:
    vector<int> v;
    vector<vector<int>> ans;
    vector<vector<int>> subsets(vector<int>& nums) {
        int n = nums.size();
        for(int mask = 0; mask < 1<<n; mask++){
            v.clear();
            for(int i = 0; i < n; i++){
                if(mask & (1<<i)){
                    v.push_back(nums[i]);
                }
            }
            ans.push_back(v);
        }
        return ans;
    }
};
```

*Method 2: Loop enumeration.*

```c++
class Solution {
public:
    vector<int> v;
    vector<vector<int>> ans;
    vector<vector<int>> subsets(vector<int>& nums) {
        v.clear();
        ans.clear();
        ans.push_back(v);
        for(int val: nums){
            int size = ans.size();
            for(int i = 0; i < size; i++){
                vector<int> newSub = ans[i];
                newSub.push_back(val);
                ans.push_back(newSub);
            }
        }
        return ans;
    }
};
```

*Method 3: Recursive enumeration.*

```c++
class Solution {
public:
    vector<vector<int>> ans;
    void recursion(vector<int> &nums, int i, vector<vector<int>> &ans){
        if(i >= nums.size()) return;
        int size = ans.size();
        for(int j = 0; j < size; j++) {
            vector<int> newSub = ans[j];
            newSub.push_back(nums[i]);
            ans.push_back(newSub);
        }
        recursion(nums, i + 1, ans);
    }
    vector<vector<int>> subsets(vector<int> &nums) {
        ans.clear();
        vector<int> temp = {};
        ans.push_back(temp);
        recursion(nums, 0, ans);
        return ans;
    }
};
```

*Method 4: Preorder traversal of the state-space tree.*

```c++
class Solution {
private:
    vector<vector<int>> ans;
public:
    void preOrder(vector<int>& nums, int depth, vector<int>& subset) {
        if(depth >= nums.size()){
            ans.push_back(subset);
        } else{
            subset.push_back(nums[depth]);
            preOrder(nums, depth + 1, subset);
            subset.pop_back();
            preOrder(nums, depth + 1, subset);
        }
    }
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<int> subset;
        subset.clear();
        ans.clear();
        preOrder(nums, 0, subset);
        return ans;
    }
};
```

## Tips

Most of the time, we are immersed in algorithm exercise for the interview, so some key points should be noted:  
- Quickly locate the related knowledge of the problem, and match the **General Template** to the knowledge, sometimes it may be necessary to do special treatment based on the specific situation.
- Throw the feasible solution first, not the optimal solution! Solve first, then optimize!
- The code style should be unified, and be familiar with the code specifications of various language.
  - Naming is as simple and clear as possible, try not to use numeric name such as: i1、node1、a1、b2.
- Summary of common errors
  - When accessing subscripts, you cannot access out-of-bounds.
  - Run time error caused by `NULL` value.

## 练习

- [ ] [strStr](https://leetcode-cn.com/problems/implement-strstr/)
- [ ] [subsets](https://leetcode-cn.com/problems/subsets/)
