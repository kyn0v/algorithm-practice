# Binary Tree

## Key Points

### Binary Tree Traversals

**Preorder Traversal**：**Visit root node first**，then traverse the left subtree in preorder, and finally traverse the right subtree in preorder.  
**Inorder Traversal**：Traverse the left subtree in inorder first, **then visit root node**, and finally traverse the right subtree in inorder.  
**Postorder Traversal**：Traverse the left subtree in postorder first, then traverse the right subtree, **and finally visit root node**.

Note:

- Determine the traversal type according to the order of node access  
- The left subtree is accessed prior to the right subtree.

```c++
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};
```
#### Recursive implement of preorder traversal

```c++
vector<int> result;
vector<int> preorderTraversal(TreeNode *root) {
    if(root){
        result.push_back(root->val);
        preorderTraversal(root->left);
        preorderTraversal(root->right);
    }
    return result;
}
```

#### Non-recursive implement of preorder traversal

```c++
vector<int> preorderTraversal(TreeNode* root) {
    vector<int> result;
    TreeNode *cur = root;
    stack<TreeNode *> st;
    while(cur || !st.empty()){
        while(cur){
            st.push(cur);
            result.push_back(cur->val);
            cur=cur->left;
        }
        cur = st.top()->right;
        st.pop();
    }
    return result;
}
```

#### Non-recursive implement of inorder traversal

```c++
vector<int> inorderTraversal(TreeNode *root) {
    vector<int> result;
    TreeNode *cur = root;
    stack<TreeNode *> st;
    while(!st.empty() || cur){
        while(cur){
            st.push(cur);
            cur=cur->left;
        }
        result.push_back(st.top()->val);
        cur = st.top()->right;
        st.pop();
    }
    return result;
}
```

#### Non-recursive implement of postorder traversal

```c++
vector<int> postorderTraversal(TreeNode* root) {
	vector<int> result;
	TreeNode *cur = root;
	TreeNode *pre = NULL;
	stack<TreeNode*> st;
	while (cur || !st.empty()) {
		if (cur) {
			st.push(cur);
			cur = cur->left;
		}
		else {
			cur = st.top();
			if (cur->right && cur->right != pre) {
				cur = cur->right;
			}
			else {
				result.push_back(cur->val);
				st.pop();
				pre = cur;
				cur = NULL;
			}
		}
	}
	return result;
}	
```

Note:

- As for postorder traversal, the root node must pop up after the right node pops up.

#### DFS (top-down)

```c++
vector<int> preorderTraversal(Treenode *root) {
    vector<int> result;
    DFS(root, result);
    return result;
}

void DFS(Treenode *root, vector<int> result) {
    if (!root) {
        return;
    }
    result.push_back(root->val);
    DFS(root->left, result);
    DFS(root->right, result);
}
```

#### DFS (bottom-up)

```c++
vector<int> preorderTraversal(Treenode *root) {
    return DFS(root);
}

vector<int> DFS(Treenode *root) {
    vector<int> result;
    if (!root) {
        return result;
    }
    // Divide
    vector<int> left = DFS(root->left);
    vector<int> right = DFS(root->right);
    // Conquer
    result.push_back(root->val);
    result.insert(result.end(), left.begin(), left.end());
    result.insert(result.end(), right.begin(), right.end());
    return result;
}
```

Note:

- Difference between top-down and bottom-up: the former generally passes the result though the parameters of pointer type, while the latter returns the result recursively and merges the final result in the end.

#### BFS (level order traversal)

```c++
vector<int> BFS(Treenode *root){
    vector<int> result;
    if(!root){
        return result;
    }
    queue<Treenode *> q;
    q.push(root);
    while(!q.empty()){
        root = q.front();
        result.push_back(root->val);
        if(root->left){
            q.push(root->left);
        }
        if(root->right){
            q.push(root->right);
        }
        q.pop();
    }
    return result;
}
```

### Application of the Divide-and-conquer Algorithm

#### Applicable scenarios

- Quicksort
- Mergesort
- Binary tree related problems

#### Template of divide-and-conquer algorithm

- Set the stop condition of recursion
- Divide the problem into a number of subproblems, and conquer the subproblem by solving them recursively 
- Combine the subsolution into the final result

```c++
ResultType traversal(TreeNode *root) {
    // null or leaf
    if(!root) {
        // do something and return
    }

    // Divide
    ResultType left = traversal(root->left)
    ResultType right = traversal(root->right)

    // Conquer
    ResultType result = merge(left, right)

    return result
}
```

#### Typical example

*1. binary tree traversal*

```c++
vector<int> preorderTraversal(Treenode *root) { 
    return divedeAndConquer(root); 
}

vector<int> divideAndConquer(Treenode *root) {
    vector<int> result;
    if (!root) {
        return result;
    }
    // Divide
    vector<int> left = divideAndConquer(root->left);
    vector<int> right = divideAndConquer(root->right);
    // Conquer
    result.push_back(root->val);
    result.insert(result.end(), left.begin(), left.end());
    result.insert(result.end(), right.begin(), right.end());
    return result;
}
```

*2. Mergesort*  

```c++
void Merge(int a[], int low, int mid, int high) {
    int len = high - low + 1;
    int* b = new int[len];
    int i = low, j = mid + 1, idx = 0;
    while (i <= mid && j <= high) {
        b[idx++] = a[i] < a[j] ? a[i++] : a[j++];
    }
    while (i <= mid) {
        b[idx++] = a[i++];
    }
    while (j <= high) {
        b[idx++] = a[j++];
    }
    for (int k = 0; k <= (len - 1); k++) {
        a[low + k] = b[k];
    }
    delete[] b;
}

void MergeSort(int a[], int low, int high) {
    if (low < high) {
        int mid = (low + high) / 2;
        MergeSort(a, low, mid);
        MergeSort(a, mid + 1, high);
        Merge(a, low, mid, high);
    }
}
```

*3. Quicksort*  

```c++
int Partition(int a[], int low, int high) {
    int pivot = a[low];
    int m = low;
    for (int k = low + 1; k <= high; k++) {
        if (a[k] < pivot) {
            m++;
            swap(a[k], a[m]);
        }
    }
    swap(a[low], a[m]);
    return m;
}

void QuickSort(int a[], int low, int high) {
    if(low < high){
        int pivot = Partition(a, low, high);
        QuickSort(a, low, pivot - 1);
        QuickSort(a, pivot + 1, high);
    }
}
```

Note：

- Quicksort swaps elements in place, and doesn't merge elements.
- The index passed in recursion is an existing index (such as: 0, lenth-1, etc.), and it may cause a crash if the index crosses the boundary.

#### Typical question

*1. maximum-depth-of-binary-tree*

- [x] [maximum-depth-of-binary-tree](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)

> Given the root of a binary tree, return its maximum depth.

```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        // stop condition
        if(!root){
            return 0;
        }
        // divide
        int l_depth = maxDepth(root->left);
        int r_depth = maxDepth(root->right);
        // conquer
        return max(l_depth, r_depth) + 1;
    }
};
```

*2. balanced-binary-tree*

- [x] [balanced-binary-tree](https://leetcode-cn.com/problems/balanced-binary-tree/)

> Given a binary tree, determine if it is height-balances

```c++
class Solution1 {
public:
    int maxDepth(TreeNode* root) {
        if(!root){
            return 0;
        }
        int left = maxDepth(root->left);
        int right = maxDepth(root->right);
        if(left == -1 || right == -1 || abs(left - right) > 1){
            return -1;
        }
        return max(left, right) + 1;
    }
    bool isBalanced(TreeNode* root) {
        if(maxDepth(root) == -1){
            return false;
        }
        return true;
    }
};

class Solution2 {
public:
    int height(TreeNode* root) {
        if(!root){
            return 0;
        }
        return max(height(root->left), height(root->right)) + 1;
    }
    bool isBalanced(TreeNode* root) {
        if(!root){
            return true;
        } else{
            return (abs(height(root->left) - height(root->right)) <= 1 && isBalanced(root->left) && isBalanced(root->right));
        }
    }
};
```

#### binary-tree-maximum-path-sum

- [x] [binary-tree-maximum-path-sum](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)

> Given the root a binary tree, return the maximum path sum of any path.

```c++
class Solution1 {
    struct ResultType{
        int single_path;    // store the maximum value of single path
        int max_path;   // store the maximum value of all path
    };
public:
    ResultType helper(TreeNode *root){
        // stop condition
        if(!root){
            return {0, INT_MIN};
        }
        // divide
        ResultType left =  helper(root->left);
        ResultType right = helper(root->right);
        // conquer
        ResultType res;
        res.single_path = (left.single_path>right.single_path)? max((left.single_path+root->val), 0):max((right.single_path+root->val), 0);
        int maxpath = max(left.max_path, right.max_path);
        res.max_path = max(maxpath, left.single_path+right.single_path+root->val);
        return res;
    }
    int maxPathSum(TreeNode* root) {
        ResultType res = helper(root);
        return res.max_path;
    }
};

class Solution2 {
private:
    int res = INT_MIN;
public:
    int helper(TreeNode* root) {
        if(!root){
            return 0;
        }
        // divide
        int left = max(helper(root->left), 0);
        int right = max(helper(root->right), 0);
        // conquer
        int maxpath = left + right + root->val;
        res = max(maxpath, res);
        int singlepath = max(left, right) + root->val;
        return singlepath;
    }
    int maxPathSum(TreeNode* root) {
        helper(root);
        return res;
    }
};
```

#### lowest-common-ancestor-of-a-binary-tree

[lowest-common-ancestor-of-a-binary-tree](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)

> 给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

思路：分治法，有左子树的公共祖先或者有右子树的公共祖先，就返回子树的祖先，否则返回根节点

```go
func lowestCommonAncestor(root, p, q *TreeNode) *TreeNode {
    // check
    if root == nil {
        return root
    }
    // 相等 直接返回root节点即可
    if root == p || root == q {
        return root
    }
    // Divide
    left := lowestCommonAncestor(root.Left, p, q)
    right := lowestCommonAncestor(root.Right, p, q)


    // Conquer
    // 左右两边都不为空，则根节点为祖先
    if left != nil && right != nil {
        return root
    }
    if left != nil {
        return left
    }
    if right != nil {
        return right
    }
    return nil
}
```

### BFS 层次应用

#### binary-tree-level-order-traversal

[binary-tree-level-order-traversal](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)

> 给你一个二叉树，请你返回其按  **层序遍历**  得到的节点值。 （即逐层地，从左到右访问所有节点）

思路：用一个队列记录一层的元素，然后扫描这一层元素添加下一层元素到队列（一个数进去出来一次，所以复杂度 O(logN)）

```go
func levelOrder(root *TreeNode) [][]int {
	result := make([][]int, 0)
	if root == nil {
		return result
	}
	queue := make([]*TreeNode, 0)
	queue = append(queue, root)
	for len(queue) > 0 {
		list := make([]int, 0)
        // 为什么要取length？
        // 记录当前层有多少元素（遍历当前层，再添加下一层）
		l := len(queue)
		for i := 0; i < l; i++ {
            // 出队列
			level := queue[0]
			queue = queue[1:]
			list = append(list, level.Val)
			if level.Left != nil {
				queue = append(queue, level.Left)
			}
			if level.Right != nil {
				queue = append(queue, level.Right)
			}
		}
		result = append(result, list)
	}
	return result
}
```

#### binary-tree-level-order-traversal-ii

[binary-tree-level-order-traversal-ii](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)

> 给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

思路：在层级遍历的基础上，翻转一下结果即可

```go
func levelOrderBottom(root *TreeNode) [][]int {
    result := levelOrder(root)
    // 翻转结果
    reverse(result)
    return result
}
func reverse(nums [][]int) {
	for i, j := 0, len(nums)-1; i < j; i, j = i+1, j-1 {
		nums[i], nums[j] = nums[j], nums[i]
	}
}
func levelOrder(root *TreeNode) [][]int {
	result := make([][]int, 0)
	if root == nil {
		return result
	}
	queue := make([]*TreeNode, 0)
	queue = append(queue, root)
	for len(queue) > 0 {
		list := make([]int, 0)
        // 为什么要取length？
        // 记录当前层有多少元素（遍历当前层，再添加下一层）
		l := len(queue)
		for i := 0; i < l; i++ {
            // 出队列
			level := queue[0]
			queue = queue[1:]
			list = append(list, level.Val)
			if level.Left != nil {
				queue = append(queue, level.Left)
			}
			if level.Right != nil {
				queue = append(queue, level.Right)
			}
		}
		result = append(result, list)
	}
	return result
}
```

#### binary-tree-zigzag-level-order-traversal

[binary-tree-zigzag-level-order-traversal](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)

> 给定一个二叉树，返回其节点值的锯齿形层次遍历。Z 字形遍历

```go
func zigzagLevelOrder(root *TreeNode) [][]int {
	result := make([][]int, 0)
	if root == nil {
		return result
	}
	queue := make([]*TreeNode, 0)
	queue = append(queue, root)
	toggle := false
	for len(queue) > 0 {
		list := make([]int, 0)
		// 记录当前层有多少元素（遍历当前层，再添加下一层）
		l := len(queue)
		for i := 0; i < l; i++ {
			// 出队列
			level := queue[0]
			queue = queue[1:]
			list = append(list, level.Val)
			if level.Left != nil {
				queue = append(queue, level.Left)
			}
			if level.Right != nil {
				queue = append(queue, level.Right)
			}
		}
		if toggle {
			reverse(list)
		}
		result = append(result, list)
		toggle = !toggle
	}
	return result
}
func reverse(nums []int) {
	for i := 0; i < len(nums)/2; i++ {
		t := nums[i]
		nums[i] = nums[len(nums)-1-i]
		nums[len(nums)-1-i] = t
	}
}
```

### 二叉搜索树应用

#### validate-binary-search-tree

[validate-binary-search-tree](https://leetcode-cn.com/problems/validate-binary-search-tree/)

> 给定一个二叉树，判断其是否是一个有效的二叉搜索树。

思路 1：中序遍历，检查结果列表是否已经有序

思路 2：分治法，判断左 MAX < 根 < 右 MIN

```go
// v1
func isValidBST(root *TreeNode) bool {
    result := make([]int, 0)
    inOrder(root, &result)
    // check order
    for i := 0; i < len(result) - 1; i++{
        if result[i] >= result[i+1] {
            return false
        }
    }
    return true
}

func inOrder(root *TreeNode, result *[]int)  {
    if root == nil{
        return
    }
    inOrder(root.Left, result)
    *result = append(*result, root.Val)
    inOrder(root.Right, result)
}


```

```go
// v2分治法
type ResultType struct {
	IsValid bool
    // 记录左右两边最大最小值，和根节点进行比较
	Max     *TreeNode
	Min     *TreeNode
}

func isValidBST2(root *TreeNode) bool {
	result := helper(root)
	return result.IsValid
}
func helper(root *TreeNode) ResultType {
	result := ResultType{}
	// check
	if root == nil {
		result.IsValid = true
		return result
	}

	left := helper(root.Left)
	right := helper(root.Right)

	if !left.IsValid || !right.IsValid {
		result.IsValid = false
		return result
	}
	if left.Max != nil && left.Max.Val >= root.Val {
		result.IsValid = false
		return result
	}
	if right.Min != nil && right.Min.Val <= root.Val {
		result.IsValid = false
		return result
	}

	result.IsValid = true
    // 如果左边还有更小的3，就用更小的节点，不用4
    //  5
    // / \
    // 1   4
    //      / \
    //     3   6
	result.Min = root
	if left.Min != nil {
		result.Min = left.Min
	}
	result.Max = root
	if right.Max != nil {
		result.Max = right.Max
	}
	return result
}
```

#### insert-into-a-binary-search-tree

[insert-into-a-binary-search-tree](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)

> 给定二叉搜索树（BST）的根节点和要插入树中的值，将值插入二叉搜索树。 返回插入后二叉搜索树的根节点。

思路：找到最后一个叶子节点满足插入条件即可

```go
// DFS查找插入位置
func insertIntoBST(root *TreeNode, val int) *TreeNode {
    if root == nil {
        root = &TreeNode{Val: val}
        return root
    }
    if root.Val > val {
        root.Left = insertIntoBST(root.Left, val)
    } else {
        root.Right = insertIntoBST(root.Right, val)
    }
    return root
}
```

## 总结

- 掌握二叉树递归与非递归遍历
- 理解 DFS 前序遍历与分治法
- 理解 BFS 层次遍历

## 练习

- [ ] [maximum-depth-of-binary-tree](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)
- [ ] [balanced-binary-tree](https://leetcode-cn.com/problems/balanced-binary-tree/)
- [ ] [binary-tree-maximum-path-sum](https://leetcode-cn.com/problems/binary-tree-maximum-path-sum/)
- [ ] [lowest-common-ancestor-of-a-binary-tree](https://leetcode-cn.com/problems/lowest-common-ancestor-of-a-binary-tree/)
- [ ] [binary-tree-level-order-traversal](https://leetcode-cn.com/problems/binary-tree-level-order-traversal/)
- [ ] [binary-tree-level-order-traversal-ii](https://leetcode-cn.com/problems/binary-tree-level-order-traversal-ii/)
- [ ] [binary-tree-zigzag-level-order-traversal](https://leetcode-cn.com/problems/binary-tree-zigzag-level-order-traversal/)
- [ ] [validate-binary-search-tree](https://leetcode-cn.com/problems/validate-binary-search-tree/)
- [ ] [insert-into-a-binary-search-tree](https://leetcode-cn.com/problems/insert-into-a-binary-search-tree/)
