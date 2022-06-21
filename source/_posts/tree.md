#### 1、二叉树的前序遍历: 中左右

给你二叉树的根节点 root ，返回它节点值的 前序 遍历。

```js
// 前序遍历:
var preorderTraversal = function (root, res = []) {
  if (!root) return res;
  res.push(root.val);
  preorderTraversal(root.left, res);
  preorderTraversal(root.right, res);
  return res;
};
```

```js
// 递归：可以使用闭包，也可以另写一个函数，然后传参root,res
var preorderTraversal = function (root) {
  let res = [];
  const dfs = function (root) {
    if (root === null) return;
    //先序遍历所以从父节点开始
    res.push(root.val);
    //递归左子树
    dfs(root.left);
    //递归右子树
    dfs(root.right);
  };
  //只使用一个参数 使用闭包进行存储结果
  dfs(root);
  return res;
};
// 非递归：中左右
var preorderTraversal = function (root) {
  if (!root) {
    return [];
  }
  const res = [];
  const stack = [root];
  while (stack.length) {
    const node = stack.pop();
    res.push(node.val);
    node.right && stack.push(node.right);
    node.left && stack.push(node.left);
  }
  return res;
};
```

#### 2、二叉树的中序遍历: 左中右

给定一个二叉树的根节点 root ，返回 它的 中序 遍历

```js
// 中序遍历:
var inorderTraversal = function (root, res = []) {
  if (!root) return res;
  inorderTraversal(root.left, res);
  res.push(root.val);
  inorderTraversal(root.right, res);
  return res;
};
```

```js
// 递归
let inOrder = (root, res) => {
  if (!root) {
    return res;
  }
  inOrder(root.left, res);
  res.push(root.val);
  inOrder(root.right, res);
  return res;
};
var inorderTraversal = function (root) {
  if (!root) {
    return [];
  }
  let res = [];
  return inOrder(root, res);
};
// 非递归: 使用栈的话，左中右，循环找到最左边
var inorderTraversal = function (root) {
  if (!root) {
    return [];
  }
  let res = [];
  const stack = [];
  let node = root;
  while (node || stack.length) {
    while (node) {
      //循环找到最左边的
      stack.push(node);
      node = node.left;
    }
    node = stack.pop(); //出栈
    res.push(node.val);
    node = node.right; //依次找每个节点的右子树
  }
  return res;
};
```

#### 3、二叉树的后序遍历：左右中

给你一棵二叉树的根节点 root ，返回其节点值的 后序遍历 。

```js
// 后序遍历:
var postorderTraversal = function (root, res = []) {
  if (!root) return res;
  postorderTraversal(root.left, res);
  postorderTraversal(root.right, res);
  res.push(root.val);
  return res;
};
```

```js
// 递归
let postOrder = (root, res) => {
  if (!root) {
    return res;
  }
  postOrder(root.left, res);
  postOrder(root.right, res);
  res.push(root.val);
  return res;
};
var postorderTraversal = function (root) {
  if (!root) {
    return [];
  }
  let res = [];
  return postOrder(root, res);
};

// 非递归1: 使用stack以及一个prev参数去保证不重复走右子树
var postorderTraversal = function (root) {
  if (!root) {
    return [];
  }
  let res = [];
  let stack = [];
  let preV = null; // 用来判断右子树是否已经走过了
  while (root || stack.length) {
    while (root) {
      //将左子树都进栈
      stack.push(root);
      root = root.left;
    }
    root = stack[stack.length - 1]; // 先不出栈，否则就丢失一个
    if (root.right === null || root.right === preV) {
      let node = stack.pop();
      res.push(node.val);
      preV = node;
      root = null;
    } else {
      root = root.right;
    }
  }
  return res;
};
// 非递归2: 先前序遍历，然后反转
var postorderTraversal = function (root) {
  if (!root) {
    return [];
  }
  let res = [];
  let stack = [root];
  while (stack.length) {
    let node = stack.pop();
    res.push(node.val);
    node.left && stack.push(node.left);
    node.right && stack.push(node.right);
  }
  return res.reverse();
};
```

#### 4、二叉树的层序遍历

给你二叉树的根节点 root ，返回其节点值的 层序遍历 。 （即逐层地，从左到右访问所有节点）。

```js
var levelOrder = function (root) {
  if (!root) {
    return [];
  }
  let res = [];
  let queue = [root];
  while (queue.length) {
    let count = queue.length; //这一层有多少个节点
    let item = []; //保存每一层的节点值
    for (let i = 0; i < count; i++) {
      let node = queue.shift();
      item.push(node.val);
      node.left && queue.push(node.left);
      node.right && queue.push(node.right);
    }
    res.push(item);
  }
  return res;
};
```

#### 5、二叉树的锯齿形层序遍历

给你二叉树的根节点 root ，返回其节点值的 锯齿形层序遍历 。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

```js
var zigzagLevelOrder = function (root) {
  if (!root) {
    return [];
  }
  let res = [];
  let queue = [root];
  let isSortLeft = true; //判断是否是从左输入
  while (queue.length) {
    let count = queue.length;
    let item = [];
    for (let i = 0; i < count; i++) {
      let node = queue.shift();
      if (isSortLeft) {
        item.push(node.val);
      } else {
        item.unshift(node.val);
      }
      node.left && queue.push(node.left);
      node.right && queue.push(node.right);
    }
    isSortLeft = !isSortLeft;
    res.push(item);
  }
  return res;
};
```

#### 6、求二叉树的最大深度

输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

```js
//深度遍历，还可以使用广度优选遍历
var maxDepth = function (root) {
  if (!root) {
    return 0;
  }
  if (!root.left && !root.right) {
    return 1;
  }
  return Math.max(maxDepth(root.left), maxDepth(root.right)) + 1;
};
```

#### 7、二叉搜索树中的第 K 大节点

给定一棵二叉搜索树，请找出其中第 k 大的节点的值。

```js
//二叉搜索树的中序遍历是生序的，逆中序遍历就是从大到小输出，第k-1的值就是第k大的数，不用使用数组的形式。
var kthLargest = function (root, k) {
  if (k <= 0 || !root) {
    return 0;
  }
  let max = 0; // 使用一个变量保存第K个值，担心遍历时导致返回的结果变化。
  const DFS = (node) => {
    //闭包
    // 当遇到空节点时 递归终止
    if (!node) return;
    DFS(node.right);
    // 当遍历k次后 终止查找并且设置最大值max
    if (!--k) return (max = node.val);
    DFS(node.left);
  };
  DFS(root);
  return max;
};
```

#### 8、路径总和 2

给你二叉树的根节点 root 和一个整数目标和 targetSum ，找出所有 从根节点到叶子节点 路径总和等于给定目标和的路径。

叶子节点 是指没有子节点的节点。

```js
// 因为要遍历整颗树，所以不需要返回值
let DFS = (node, count, item, res) => {
  // 如果是叶子节点，并符合target，加到res数组中
  if (!node.left && !node.right && count === 0) {
    res.push([...item]);
    return;
  }
  // 如果是叶子节点了，不符合target，直接返回
  if (!node.left && !node.right) {
    return;
  }
  // 前序遍历
  if (node.left) {
    item.push(node.left.val);
    DFS(node.left, count - node.left.val, item, res);
    item.pop();
  }
  if (node.right) {
    item.push(node.right.val);
    DFS(node.right, count - node.right.val, item, res);
    item.pop();
  }
};
var pathSum = function (root, targetSum) {
  let res = [];
  if (!root) {
    return res;
  }
  let item = [root.val];
  let count = targetSum - root.val;
  DFS(root, count, item, res);
  return res;
};
```

#### 9、二叉树的最近公共祖先

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

最近公共祖先：如果 root 是 p,q 的公共祖先，并且 root.left 和 root.right 都不是 p,q 的公共祖先。

后序遍历：

1） p,q 在 root 的子树上，且分别在两侧

2）p 是 root，q 在 root 的子树上

3）q 是 root，p 在 root 的子树上

```js
var lowestCommonAncestor = function (root, p, q) {
  if (!root || root === p || root === q) return root; //当root为null或等于p,q就返回root
  const left = lowestCommonAncestor(root.left, p, q);
  const right = lowestCommonAncestor(root.right, p, q);
  if (!left) return right; // 左子树找不到，返回右子树， 不用考虑左右子树都不存在的情况下，因为左右子树都不存在，也返回null，和这种情况一样
  if (!right) return left; // 右子树找不到，返回左子树
  return root;
};
//返回结果
//1.当left和right都为空，说明左右都不包含p,q，返回null
//2.当left和right都不为空，说明左右分别包含p,q，返回root
//3.当 left 为空 ，right 不为空 ：p,q 都不在 root 的左子树中，直接返回 right 。
// 具体可分为两种情况：
//  p,q 其中一个在 root的右子树中，此时 right 指向 p（假设为 p ）；
//  p,q 两节点都在 root的右子树中，此时的 right 指向最近公共祖先节点 ；
//当 left 不为空 ， right 为空 ：与情况 3. 同理
```

#### 10、重建二叉树：输入前序和中序遍历

输入某二叉树的前序遍历和中序遍历的结果，请构建该二叉树并返回其根节点。
假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

```js
//前序遍历第一个数是父节点，然后在中序遍历中找到这个节点，前面的是左子树，后面的是右子树。
var buildTree = function (preorder, inorder) {
  if (
    preorder.length === 0 ||
    inorder.length === 0 ||
    preorder.length !== inorder.length
  ) {
    return null;
  }
  let node = new TreeNode(preorder[0]);
  for (let i = 0; i < inorder.length; i++) {
    if (inorder[i] === node.val) {
      node.left = buildTree(preorder.slice(1, i + 1), inorder.slice(0, i));
      node.right = buildTree(preorder.slice(i + 1), inorder.slice(i + 1));
      break;
    }
  }
  return node;
};
```

#### 11、二叉树的右视图

给定一个二叉树的 根节点 root，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

```js
// 广度优先遍历
var rightSideView = function (root) {
  let res = [];
  if (!root) {
    return res;
  }
  let queue = [root];
  while (queue.length) {
    let count = queue.length;
    for (let i = 0; i < count; i++) {
      let node = queue.shift();
      if (i === count - 1) {
        res.push(node.val);
      }
      node.left && queue.push(node.left);
      node.right && queue.push(node.right);
    }
  }
  return res;
};
// 深度优先遍历
var rightSideView = function (root) {
  let res = [];
  if (!root) {
    return res;
  }
  let dfs = (step, root) => {
    // 根据深度和res的长度去判断是否该层存在，
    if (res.length === step) {
      res.push(root.val);
    }
    root.right && dfs(step + 1, root.right);
    root.left && dfs(step + 1, root.left);
  };
  dfs(0, root);
  return res;
};
```

#### 12、二叉树展开为链表

给你二叉树的根结点 root ，请你将它展开为一个单链表：

展开后的单链表应该同样使用 TreeNode ，其中 right 子指针指向链表中下一个结点，而左子指针始终为 null 。
展开后的单链表应该与二叉树 先序遍历 顺序相同。

```js
// 先序遍历即可
var flatten = function (root) {
  if (!root) {
    return root;
  }
  let stack = [root];
  while (stack.length) {
    let node = stack.pop();
    node.right && stack.push(node.right);
    node.left && stack.push(node.left);
    node.right = node.left
      ? node.left
      : stack.length
      ? stack[stack.length - 1]
      : null;
    node.left = null;
  }
  return root;
};
```

#### 13、将有序数组转换为二叉搜索树

给你一个整数数组 nums ，其中元素已经按 升序 排列，请你将其转换为一棵 高度平衡 二叉搜索树。

高度平衡 二叉树是一棵满足「每个节点的左右两个子树的高度差的绝对值不超过 1 」的二叉树。

```js
// 二分查找一起，每次把中间的位置放在树中间的位置
var sortedArrayToBST = function (nums) {
  let len = nums.length;
  if (len < 1) {
    return null;
  }
  let left = 0;
  let right = len - 1;
  let mid = left + ((right - left) >> 1);
  let node = new TreeNode(nums[mid]);
  node.left = sortedArrayToBST(nums.slice(0, mid));
  node.right = sortedArrayToBST(nums.slice(mid + 1));
  return node;
};
```

#### 14、二叉搜索树的最小绝对差

给你一个二叉搜索树的根节点 root ，返回树中任意两不同节点值之间的最小差值 。

差值是一个正数，其数值等于两值之差的绝对值。

```js
// 中序遍历，最小值一定两个数字之间
var getMinimumDifference = function (root) {
  if (!root || (!root.left && !root.right)) {
    return 0;
  }
  let min = Number.MAX_VALUE;
  let pre = -1; // 记录上一次的值
  const DFS = (node) => {
    if (!node) {
      return;
    }
    DFS(node.left);
    if (pre === -1) {
      pre = node.val;
    } else {
      min = Math.min(node.val - pre, min);
      pre = node.val;
    }
    DFS(node.right);
  };
  DFS(root);
  return min;
};
```

#### 15、最大二叉树

给定一个不重复的整数数组  nums 。  最大二叉树   可以用下面的算法从  nums 递归地构建:

创建一个根节点，其值为  nums 中的最大值。
递归地在最大值   左边   的   子数组前缀上   构建左子树。
递归地在最大值 右边 的   子数组后缀上   构建右子树。
返回  nums 构建的 最大二叉树 。

```text
nums = [3,2,1,6,0,5]
输出：[6,3,5,null,2,0,null,null,1]
解释：递归调用如下所示：
- [3,2,1,6,0,5] 中的最大值是 6 ，左边部分是 [3,2,1] ，右边部分是 [0,5] 。
    - [3,2,1] 中的最大值是 3 ，左边部分是 [] ，右边部分是 [2,1] 。
        - 空数组，无子节点。
        - [2,1] 中的最大值是 2 ，左边部分是 [] ，右边部分是 [1] 。
            - 空数组，无子节点。
            - 只有一个元素，所以子节点是一个值为 1 的节点。
    - [0,5] 中的最大值是 5 ，左边部分是 [0] ，右边部分是 [] 。
        - 只有一个元素，所以子节点是一个值为 0 的节点。
        - 空数组，无子节点。
```

```js
// 先序遍历，不过每次一个for循环去找最大值
var constructMaximumBinaryTree = function (nums) {
  const BuildTree = (arr, left, right) => {
    if (left > right) return null;
    let maxValue = -1;
    let maxIndex = -1;
    for (let i = left; i <= right; ++i) {
      if (arr[i] > maxValue) {
        maxValue = arr[i];
        maxIndex = i;
      }
    }
    let root = new TreeNode(maxValue);
    root.left = BuildTree(arr, left, maxIndex - 1);
    root.right = BuildTree(arr, maxIndex + 1, right);
    return root;
  };
  let root = BuildTree(nums, 0, nums.length - 1);
  return root;
};
```

#### 16、把二叉搜索树转换为累加树

给出二叉 搜索 树的根节点，该树的节点值各不相同，请你将其转换为累加树（Greater Sum Tree），使每个节点 node  的新值等于原树中大于或等于  node.val  的值之和。

提醒一下，二叉搜索树满足下列约束条件：

节点的左子树仅包含键 小于 节点键的节点。
节点的右子树仅包含键 大于 节点键的节点。
左右子树也必须是二叉搜索树。

```js
// 反向中序遍历即可，不需要数组计算值
const convertBST = (root) => {
  let sum = 0;
  const inOrder = (root) => {
    if (root == null) {
      // 遍历到null节点，开始返回
      return;
    }
    inOrder(root.right); // 先进入右子树

    sum += root.val; // 节点值累加给sum
    root.val = sum; // 累加的结果，赋给root.val

    inOrder(root.left); // 然后才进入左子树
  };
  inOrder(root); // 递归的入口，从根节点开始
  return root;
};

// 使用了中序遍历，两个数组保留了加和的值
var convertBST = function (root) {
  if (!root) {
    return null;
  }
  let res = [];
  let i = 0;
  const DFS = (node, flag) => {
    if (!node) return;
    node.left && DFS(node.left, flag);
    if (!flag) {
      res.push(node.val);
    } else {
      node.val = rightRes[i++];
    }
    node.right && DFS(node.right, flag);
  };
  DFS(root, false);
  let len = res.length;
  let rightRes = new Array(len).fill(res[len - 1]);
  for (let i = len - 2; i >= 0; i--) {
    rightRes[i] = rightRes[i + 1] + res[i];
  }
  DFS(root, true);
  return root;
};
```

#### 17、完全二叉树的节点个数

给你一棵 完全二叉树 的根节点 root ，求出该树的节点个数。

完全二叉树 的定义如下：在完全二叉树中，除了最底层节点可能没填满外，其余每层节点数都达到最大值，并且最下面一层的节点都集中在该层最左边的若干位置。若最底层为第 h 层，则该层包含 1~ 2h  个节点。

```js
var countNodes = function (root) {
  if (!root) {
    //没有值的时候返回0
    return 0;
  }
  if (!root.left && !root.right) {
    // 没有左右子树，返回1
    return 1;
  }
  let left = 0; // 左边深度
  let right = 0; //右边深度
  let leftNode = root;
  let rightNode = root;
  while (leftNode) {
    left++;
    leftNode = leftNode.left;
  }
  while (rightNode) {
    right++;
    rightNode = rightNode.right;
  }
  if (left === right) {
    //左右深度一样，说明是满二叉树，就是用2^h-1
    return Math.pow(2, left) - 1;
  }
  return countNodes(root.left) + countNodes(root.right) + 1;
};
```

#### 18、二叉树的最小深度

给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

说明：叶子节点是指没有子节点的节点。

```js
var minDepth = function (root) {
  //根节点为空返回空
  if (!root) return 0;
  //查看一下左子树的深度
  let left = minDepth(root.left);
  //查看一下右子树的深度
  let right = minDepth(root.right);
  //如果左子树或右子树有一层为空就返回当前左（或右）子树的值加1，加零相当于没加
  if (!left || !right) return left + right + 1;
  //最后返回一个左右子树的最小值加1
  return Math.min(left, right) + 1;
};
```

#### 19. 删除二叉搜索树中的节点

给定一个二叉搜索树的根节点 root 和一个值 key，删除二叉搜索树中的  key  对应的节点，并保证二叉搜索树的性质不变。返回二叉搜索树（有可能被更新）的根节点的引用。

一般来说，删除节点可分为两个步骤：

首先找到需要删除的节点；
如果找到了，删除它。

```js
var deleteNode = function (root, key) {
  if (!root || (!root.left && !root.right && key !== root.val)) {
    return root;
  }
  let getMin = (node) => {
    // BST 最左边的就是最小的
    while (node.left) {
      node = node.left;
    }
    return node;
  };
  if (root.val === key) {
    // 左子树为空，返回右子树
    if (!root.left) {
      return root.right;
    }
    // 右子树为空，返回左子树
    if (!root.right) {
      return root.left;
    }
    // 拿到右子树的最左侧节点
    let node = getMin(root.right);
    // 将要删除节点的左子树移到右子树的最左边
    node.left = root.left;
    // 返回右子树
    return root.right;
  }
  if (root.val > key) root.left = deleteNode(root.left, key);
  if (root.val < key) root.right = deleteNode(root.right, key);
  return root;
};
```

#### 20. 平衡二叉树

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过 1，那么它就是一棵平衡二叉树。

```js
// 1. 自上而下
// 如果树是空树，返回true，否则计算树节点的左右子树深度，不超过2，再判断左右子树是否为平衡二叉树。递归
var isBalanced = function (root) {
  if (!root) {
    return true;
  }
  let left = depth(root.left);
  let right = depth(root.right);
  // 计算左子树和右子树的深度差
  // 判断左右子树是否平衡
  return (
    Math.abs(left - right) < 2 &&
    isBalanced(root.left) &&
    isBalanced(root.right)
  );
};

/**
 * 深度计算函数
 */
var depth = function (root) {
  if (!root) {
    return 0;
  } else {
    // 取左右子树中深度比较大的值作为返回结果 +1
    return Math.max(depth(root.left), depth(root.right)) + 1;
  }
};

// 2. 自下而上
// 自上而下，存在着大量的重复计算
// 对左右子树递归调用深度函数计算深度，如果左子树结果为-1，返回-1，如果右子树结果为-1，返回-1，如果左右子树的差>1, 返回-1， 否则取左右子树深度值大的+1.
// 结果为-1，就不是平衡二叉树，不为-1，就为平衡二叉树。
var isBalanced = function (root) {
  if (!root) {
    return true;
  }
  return dfs(root) !== -1;
};

var dfs = (root) => {
  if (!root) {
    return 0;
  }
  let left = dfs(root.left);
  let right = dfs(root.right);
  if (left === -1 || right === -1) {
    return -1;
  } else if (Math.abs(left - right) >= 2) {
    return -1;
  } else {
    return Math.max(left, right) + 1;
  }
};
```

#### 21. 二叉树的序列化与反序列化

序列化是将一个数据结构或者对象转换为连续的比特位的操作，进而可以将转换后的数据存储在一个文件或者内存中，同时也可以通过网络传输到另一个计算机环境，采取相反方式重构得到原数据。

请设计一个算法来实现二叉树的序列化与反序列化。这里不限定你的序列 / 反序列化算法执行逻辑，你只需要保证一个二叉树可以被序列化为一个字符串并且将这个字符串反序列化为原始的树结构。

```js
// BFS
var serialize = function (root) {
  if (!root) {
    return "";
  }
  let queue = [root];
  let res = [];
  while (queue.length) {
    let node = queue.shift();
    if (node) {
      res.push(node.val);
    } else {
      res.push("X");
    }
    node && queue.push(node.left);
    node && queue.push(node.right);
  }
  return res.join(",");
};

/**
 * Decodes your encoded data to tree.
 *
 * @param {string} data
 * @return {TreeNode}
 */
const deserialize = (data) => {
  if (data.length == 0) return null;
  if (data == "X") return null;

  const list = data.split(","); // 序列化字符串split成数组

  const root = new TreeNode(list[0]); // 获取首项，构建根节点
  const queue = [root]; // 根节点推入队列
  let cursor = 1; // 初始指向list第二项

  while (cursor < list.length) {
    // 指针越界，即扫完了序列化字符串
    const node = queue.shift(); // 考察出列的节点

    const leftVal = list[cursor]; // 它的左儿子的值
    const rightVal = list[cursor + 1]; // 它的右儿子的值

    if (leftVal != "X") {
      // 是真实节点
      const leftNode = new TreeNode(leftVal); // 创建左儿子节点
      node.left = leftNode; // 认父亲
      queue.push(leftNode); // 自己也是父亲，入列
    }
    if (rightVal != "X") {
      const rightNode = new TreeNode(rightVal);
      node.right = rightNode;
      queue.push(rightNode);
    }
    cursor += 2; // 一次考察一对儿子，指针加2
  }
  return root; // BFS结束，构建结束，返回根节点
};
```

#### 22. 二叉树的所有路径

给你一个二叉树的根节点 root ，按任意顺序 ，返回所有从根节点到叶子节点的路径。

叶子节点 是指没有子节点的节点。

```js
// 返回的路径应该是["1->2->5","1->3"]这个样的
var binaryTreePaths = function (root) {
  if (!root) {
    return [];
  }
  let res = [];
  let item = [];
  let dfs = (node) => {
    item.push(node.val);
    // 如果是叶子节点
    if (!node.left && !node.right) {
      res.push(item.join("->"));
    }
    node.left && dfs(node.left);
    node.right && dfs(node.right);
    item.pop();
  };
  dfs(root);
  return res;
};
```

#### 23. 左叶子之和

给定二叉树的根节点 root ，返回所有左叶子之和。

```js
var sumOfLeftLeaves = function (root) {
  if (!root || (!root.left && !root.right)) {
    return 0;
  }
  let sum = 0; //保存最后的总和
  let dfs = (node) => {
    //递归调用
    if (!node) {
      return;
    }
    dfs(node.left);
    dfs(node.right);
    if (
      node.left !== null &&
      node.left.left === null &&
      node.left.right === null
    ) {
      // 只有用父节点才能判断出来子节点是不是左孩子
      sum += node.left.val;
    }
  };
  dfs(root);
  return sum;
};
```

#### 24. 找到左下角的值

给定一个二叉树的 根节点 root，请找出该二叉树的 最底层 最左边 节点的值。

```js
var findBottomLeftValue = function (root) {
  if (!root) {
    return -1;
  }
  if (!root.left && !root.right) {
    return root.val;
  }
  let queue = [root];
  let res = 0;
  while (queue.length) {
    let count = queue.length;
    for (let i = 0; i < count; i++) {
      let node = queue.shift();
      if (i === 0) {
        res = node.val;
      }
      node.left && queue.push(node.left);
      node.right && queue.push(node.right);
    }
  }
  return res;
};
```

#### 25. 修剪二叉搜索树

给你二叉搜索树的根节点 root ，同时给定最小边界 low 和最大边界 high。通过修剪二叉搜索树，使得所有节点的值在[low, high]中。修剪树 不应该   改变保留在树中的元素的相对结构 (即，如果没有被移除，原有的父代子代关系都应当保留)。 可以证明，存在   唯一的答案  。

所以结果应当返回修剪好的二叉搜索树的新的根节点。注意，根节点可能会根据给定的边界发生改变。

```js
var trimBST = function (root, low, high) {
  if (root === null) {
    return null;
  }
  if (root.val < low) {
    let right = trimBST(root.right, low, high);
    return right;
  }
  if (root.val > high) {
    let left = trimBST(root.left, low, high);
    return left;
  }
  root.left = trimBST(root.left, low, high);
  root.right = trimBST(root.right, low, high);
  return root;
};
```

#### 26. 二叉搜索树的最近公共祖先

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

```js
// 递归
var lowestCommonAncestor = function (root, p, q) {
  if (!root) {
    return root;
  }
  if (root.val < p.val && root.val < q.val) {
    //根据二叉搜索树是一个升序排列，如果root小于p,q，在右子树上
    return lowestCommonAncestor(root.right, p, q);
  } else if (root.val > p.val && root.val > q.val) {
    return lowestCommonAncestor(root.left, p, q);
  } else {
    return root;
  }
};
//非递归
const lowestCommonAncestor = (root, p, q) => {
  if (!root) return null;
  // 如果相等，公共祖先就是他们本身
  if (p.val === q.val) return p;
  // 遍历root
  while (root) {
    // 当前节点值在p，q的左边，遍历右子树
    if (root.val < q.val && root.val < p.val) {
      root = root.right;
    } else if (root.val > q.val && root.val > p.val) {
      // 当前节点值在p，q的右边，遍历左子树
      root = root.left;
    } else {
      // 当前节点值在p，q的中间，那么当前节点就是公共祖先
      return root;
    }
  }
};
```

#### 27. 二叉搜索树的后序遍历序列

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。

```js
var verifyPostorder = function (postorder) {
  // 后序遍历应该是左右根
  if (postorder.length <= 2) {
    return true;
  }
  const root = postorder.pop(); // 根节点
  let i = 0;
  // 二叉搜索树，左子树的值都比根节点要小，右子树的值都比根节点要大
  // 找到左右子树的分界点
  while (postorder[i] < root) {
    i++;
  }
  let rightTree = postorder.slice(i); //右子树的节点内容
  // 右子树的节点都应该比根节点大
  const rightResult = rightTree.every((item) => item > root);
  //对左右子树递归判断。如果所有的都满足就返回true，否则返回false
  return (
    rightResult &&
    verifyPostorder(postorder.slice(0, i)) &&
    verifyPostorder(rightTree)
  );
};
```

#### 28、树的子结构

判断 B 树是不是 A 树的子结构，空树不是任何树的子树

```js
var isSubStructure = function (A, B) {
  if (!A || !B) {
    //如果A、B树有空树就返回false
    return false;
  }
  let result = false;
  if (A.val === B.val) {
    //只有A树中节点和B树中根节点值相同，才会去判断子节点
    result = compare(A, B);
  }
  result = result || isSubStructure(A.left, B) || isSubStructure(A.right, B); //否则就遍历左右子树，找到A中与B根节点相同的节点。
  return result;
};
var compare = function (parent, child) {
  if (!child) {
    //如果子树中子节点为空，返回true
    return true;
  }
  if (!parent) {
    //如果子树中节点不为空，但是父树中节点为空，返回false
    return false;
  }
  if (parent.val !== child.val) {
    return false;
  }
  return compare(parent.left, child.left) && compare(parent.right, child.right);
};
```

#### 29、合并二叉树

给你两棵二叉树： root1 和 root2 。

想象一下，当你将其中一棵覆盖到另一棵之上时，两棵树上的一些节点将会重叠（而另一些不会）。你需要将这两棵树合并成一棵新二叉树。合并的规则是：如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，不为 null 的节点将直接作为新二叉树的节点。

返回合并后的二叉树。

注意: 合并过程必须从两个树的根节点开始。

```js
// 先序遍历
var mergeTrees = function (root1, root2) {
  if (!root1) {
    return root2;
  }
  if (!root2) {
    return root1;
  }
  let root = new TreeNode(0);
  if (root1 && root2) {
    root.val = root1.val + root2.val;
  }
  root.left = mergeTrees(root1.left, root2.left);
  root.right = mergeTrees(root1.right, root2.right);
  return root;
};
```

#### 30、填充每个节点的下一个右侧节点指针

给定一个完美二叉树，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

```js
struct Node {
int val;
Node *left;
Node *right;
Node \*next;
}
```

填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL。

初始状态下，所有  next 指针都被设置为 NULL。

```js
/**
 * @param {Node} root, 可以使用层次遍历，当遍历一层的最后一个节点时，指向null
 * @return {Node}
 */
var connect = function (root) {
  if (!root) {
    return root;
  }
  let queue = [root];
  while (queue.length) {
    let count = queue.length;
    for (let i = 0; i < count; i++) {
      let node = queue.shift();
      if (i === count - 1) {
        node.next = null;
      } else {
        node.next = queue[0];
      }
      node.left && queue.push(node.left);
      node.right && queue.push(node.right);
    }
  }
  return root;
};
```

#### 8、二叉树的镜像

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

```js
// 从上到下，依次交换节点的左右节点
var mirrorTree = function (root) {
  if (!root || (!root.left && !root.right)) {
    return root;
  }
  [root.left, root.right] = [root.right, root.left];
  mirrorTree(root.left);
  mirrorTree(root.right);
  return root;
};
```
