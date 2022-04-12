#### 1、重建二叉树：输入前序和中序遍历

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

#### 2、二叉树的最近公共祖先

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

#### 3、二叉搜索树的最近公共祖先

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

#### 4、求二叉树的深度

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
