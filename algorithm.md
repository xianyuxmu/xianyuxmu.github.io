---
title:
layout: page
---


# 《剑指offer》题目集合

### 树

#### [剑指 Offer 27. 二叉树的镜像 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof/)

完成一个函数，输入一个二叉树，该函数输出它的镜像。


```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {TreeNode}
 */
var mirrorTree = function(root) {
    if(root == null) {
        return null
    }
    let temp = root.left
    root.left = mirrorTree(root.right)
    root.right = mirrorTree(temp)

    return root

};
```


#### [剑指 Offer 32 - II. 从上到下打印二叉树 II - 力扣（LeetCode）](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof/)

从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。


```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[][]}
 */
var levelOrder = function(root) {
    let res = []

    function bfs(node, level) {
        if(node == null) {
            return
        }

        if(res[level] === undefined) {
            res[level] = []
        }

        res[level].push(node.val)

        bfs(node.left, level + 1)
        bfs(node.right, level + 1)
    }

    bfs(root, 0)

    return res
};
```

#### [剑指 Offer 54. 二叉搜索树的第k大节点 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-di-kda-jie-dian-lcof/)

给定一棵二叉搜索树，请找出其中第k大的节点。


```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} k
 * @return {number}
 */
var kthLargest = function(root, k) {
    let inorderArr = []

    // 使用“右、中、左”顺序遍历
    function dfs(node) {
        if(node == null) {
            return
        }

        if(node.right !== null) {
            dfs(node.right)
        }

        inorderArr.push(node.val)

        if(node.left !== null) {
            dfs(node.left)
        }
    }

    dfs(root)
    
    return inorderArr[k - 1]
};
```


#### [剑指 Offer 55 - I. 二叉树的深度 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var maxDepth = function(root) {
    if(root == null) {
        return 0
    }
    if(root.left == null && root.right == null) {
        return 1
    }

    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right))
};
```


[剑指 Offer 55 - II. 平衡二叉树 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。


```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
var isBalanced = function(root) {

    function recur(node) {
        if(node == null) {
            return 0
        }
        let left = recur(node.left)
        if(left == -1) return -1
        let right = recur(node.right)
        if(right == -1 ) return -1

        return Math.abs(left - right) >= 2 ? -1 : Math.max(left, right) + 1
    }

    return recur(root) !== -1

};
```

[剑指 Offer 68 - I. 二叉搜索树的最近公共祖先 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-zui-jin-gong-gong-zu-xian-lcof/)

给定一个二叉搜索树, 找到该树中两个指定节点的最近公共祖先。

百度百科中最近公共祖先的定义为：“对于有根树 T 的两个结点 p、q，最近公共祖先表示为一个结点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（一个节点也可以是它自己的祖先）。”

例如，给定如下二叉搜索树:  root = [6,2,8,0,4,7,9,null,null,3,5]


```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        // 保证p小于q
        if(p.val > q.val) {
            TreeNode temp = p;
            p = q;
            q = temp;
        }

        while(root != null) {
            if(root.val < p.val) {
                // p、q都在右子树
                root = root.right;
            } else if(root.val > q.val) {
                // p、q都在左子树
                root = root.left;
            } else {
                // 这个条件中，p在root左边、q在root右边，这种情况下root必定为p、q的最近公共祖先
                break;
            }
        }

        return root;
    }
}
```

[剑指 Offer 68 - II. 二叉树的最近公共祖先 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)


```
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        if(root == null || root == p || root == q) {
            return root;
        }

        TreeNode left = lowestCommonAncestor(root.left, p, q);
        TreeNode right = lowestCommonAncestor(root.right, p, q);
        if(left == null && right == null) {
            return null;
        } else if(left == null) {
            return right;
        } else if(right == null) {
            return left;
        } else {
            return root;
        }
    }
}
```


[剑指 Offer 28. 对称的二叉树 - 力扣（LeetCode）](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof/)

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

    1
   / \
  2   2
 / \ / \
3  4 4  3
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

    1
   / \
  2   2
   \   \
   3    3


```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
var isSymmetric = function(root) {
    if(root == null) {
        return true
    }

    function recur(L, R) {
        if(L == null && R == null) {
            return true
        }

        if(L == null || R == null || L.val !== R.val) {
            return false
        }

        return recur(L.left, R.right) && recur(L.right, R.left)
    }

    return recur(root.left, root.right)

};
```

### 中等难度

[剑指 Offer 07. 重建二叉树 - 力扣（LeetCode）](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)

输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

 

例如，给出

前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：

    3
   / \
  9  20
    /  \
   15   7


```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {number[]} preorder
 * @param {number[]} inorder
 * @return {TreeNode}
 */
var buildTree = function(preorder, inorder) {
    if(!preorder.length || !inorder.length) {
        return null
    }

    const rootVal = preorder[0]
    const node = new TreeNode(rootVal)

    let i = 0 // 当前节点在inorder中的位置
    for(; i < inorder.length; ++ i) {
        if(inorder[i] === rootVal) {
            break
        }
    }

    node.left = buildTree(preorder.slice(1, i + 1), inorder.slice(0, i))
    node.right = buildTree(preorder.slice(i + 1), inorder.slice(i + 1))

    return node
};
```

[剑指 Offer 26. 树的子结构 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

例如:
给定的树 A:

     3
    / \
   4   5
  / \
 1   2
给定的树 B：

   4 
  /
 1
返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。

示例 1：

输入：A = [1,2,3], B = [3,1]
输出：false
示例 2：

输入：A = [3,4,5,1,2], B = [4,1]
输出：true



```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} A
 * @param {TreeNode} B
 * @return {boolean}
 */
var isSubStructure = function(A, B) {

    function recur(nodeA, nodeB) {
        if(nodeB == null) {
            return true
        }

        if(nodeA == null || nodeA.val !== nodeB.val) {
            return false
        }

        return recur(nodeA.left, nodeB.left) && recur(nodeA.right, nodeB.right)
    }

    return (A !== null && B !== null) && (recur(A, B) || isSubStructure(A.left, B) || isSubStructure(A.right, B))

};
```


[剑指 Offer 32 - I. 从上到下打印二叉树 - 力扣（LeetCode）](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof/)

从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

 

例如:
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回：

[3,9,20,15,7]



```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[]}
 */
var levelOrder = function(root) {
    let queue = []
    let res = []

    if(root == null) {
        return res
    }

    queue.push(root)

    while(queue.length) {
        let node = queue.shift()
        res.push(node.val)

        if(node.left !== null) {
            queue.push(node.left)
        }
        if(node.right !== null) {
            queue.push(node.right)
        }
    }

    return res
};
```



[剑指 Offer 32 - III. 从上到下打印二叉树 III - 力扣（LeetCode）](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof/)

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

 

例如:
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [20,9],
  [15,7]
]


```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number[][]}
 */
var levelOrder = function(root) {
    let queue = []
    let res = []

    if(root != null) {
        queue.push(root)
    }

    while(queue.length) {
        let temp = []
        let isEvenLine = res.length % 2 === 0 // 偶数行
        for(let i = 0, len = queue.length; i < len; ++ i) {
            let node = queue.shift()
            isEvenLine ? temp.push(node.val) : temp.unshift(node.val)
            if(node.left != null) {
                queue.push(node.left)
            }
            if(node.right != null) {
                queue.push(node.right)
            }   
        }

        res.push(temp)
    }

    return res
};
```

[剑指 Offer 34. 二叉树中和为某一值的路径 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。

 

示例:
给定如下二叉树，以及目标和 sum = 22，

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
返回:

[
   [5,4,11,2],
   [5,8,4,5]
]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} sum
 * @return {number[][]}
 */
var pathSum = function(root, sum) {
    let res = []
    let path = []

    function recur(node, tar) {
        if(node == null) return
        path.push(node.val)
        tar -= node.val
        if(tar === 0 && node.left === null && node.right === null) {
            res.push([].concat(path))
        }
        recur(node.left, tar)
        recur(node.right, tar)
        path.pop()
    }

    recur(root, sum)

    return res

};
```

### 难度困难

[剑指 Offer 37. 序列化二叉树 - 力扣（LeetCode）](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)

请实现两个函数，分别用来序列化和反序列化二叉树。

示例: 

你可以将以下二叉树：

    1
   / \
  2   3
     / \
    4   5

序列化为 "[1,2,3,null,null,4,5]"

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */

function serializeHelper(node, str) {
    if(node === null) {
        str += 'None,'
    } else {
        str = str + node.val + ','
        str = serializeHelper(node.left, str)
        str = serializeHelper(node.right, str)
    }

    return str
}

/**
 * Encodes a tree to a single string.
 *
 * @param {TreeNode} root
 * @return {string}
 */
var serialize = function(root) {
    let res = serializeHelper(root, '')
    return res
};


function deserializeHelper(treeArray) {
    if(treeArray.length && treeArray[0] === 'None') {
        treeArray.shift()
        return null
    }

    let root = new TreeNode(treeArray.shift())

    root.left = deserializeHelper(treeArray)
    root.right = deserializeHelper(treeArray)

    return root
}

/**
 * Decodes your encoded data to tree.
 *
 * @param {string} data
 * @return {TreeNode}
 */
var deserialize = function(data) {
    if(data === null) {
        return null
    }

    let res = deserializeHelper(data.split(','))

    return res
};

/**
 * Your functions will be called as such:
 * deserialize(serialize(root));
 */
```


----

## 深度优先搜索

### 简单

[剑指 Offer 55 - I. 二叉树的深度 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof/)

输入一棵二叉树的根节点，求该树的深度。从根节点到叶节点依次经过的节点（含根、叶节点）形成树的一条路径，最长路径的长度为树的深度。

例如：

给定二叉树 [3,9,20,null,null,15,7]，

    3
   / \
  9  20
    /  \
   15   7
返回它的最大深度 3 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-cha-shu-de-shen-du-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {number}
 */
var maxDepth = function(root) {
    if(root == null) {
        return 0
    }
    if(root.left == null && root.right == null) {
        return 1
    }

    return 1 + Math.max(maxDepth(root.left), maxDepth(root.right))
};
```

[剑指 Offer 55 - II. 平衡二叉树 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof/)

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

 

示例 1:

给定二叉树 [3,9,20,null,null,15,7]

    3
   / \
  9  20
    /  \
   15   7
返回 true 。

示例 2:

给定二叉树 [1,2,2,3,3,null,null,4,4]

       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
返回 false 。



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @return {boolean}
 */
var isBalanced = function(root) {

    function recur(node) {
        if(node == null) {
            return 0
        }
        let left = recur(node.left)
        if(left == -1) return -1
        let right = recur(node.right)
        if(right == -1 ) return -1

        return Math.abs(left - right) >= 2 ? -1 : Math.max(left, right) + 1
    }

    return recur(root) !== -1

};
```

### 中等难度

[剑指 Offer 34. 二叉树中和为某一值的路径 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof/)

输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。

 

示例:
给定如下二叉树，以及目标和 sum = 22，

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
返回:

[
   [5,4,11,2],
   [5,8,4,5]
]

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * @param {TreeNode} root
 * @param {number} sum
 * @return {number[][]}
 */
var pathSum = function(root, sum) {
    let res = []
    let path = []

    function recur(node, tar) {
        if(node == null) return
        path.push(node.val)
        tar -= node.val
        if(tar === 0 && node.left === null && node.right === null) {
            res.push([].concat(path))
        }
        recur(node.left, tar)
        recur(node.right, tar)
        path.pop()
    }

    recur(root, sum)

    return res

};
```

[剑指 Offer 12. 矩阵中的路径 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof/)

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。

[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

 

示例 1：

输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
示例 2：

输入：board = [["a","b"],["c","d"]], word = "abcd"
输出：false
提示：

1 <= board.length <= 200
1 <= board[i].length <= 200

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * @param {character[][]} board
 * @param {string} word
 * @return {boolean}
 */

function dfs(board, wordArr, i, j, k) {
    if(i >= board.length || i < 0 || j >= board[0].length || j < 0 || board[i][j] !== wordArr[k]) {
        return false
    }
    if(k == wordArr.length - 1) {
        return true
    }
    let temp = board[i][j]
    board[i][j] = '/' // 将当前节点设置为不可走
    let res = dfs(board, wordArr, i - 1, j, k + 1) || dfs(board, wordArr, i + 1, j, k + 1) || dfs(board, wordArr, i, j + 1, k + 1) || dfs(board, wordArr, i, j - 1, k + 1)
    board[i][j] = temp // 恢复当前节点
    return res
}
var exist = function(board, word) {
    let wordArr = word.split('')

    for(let i = 0; i < board.length; ++ i) {
        for(let j = 0; j < board[i].length; ++ j) {
            if(dfs(board, wordArr, i, j, 0)) {
                return true
            }
            
        }
    }
    return false
};
```

### 宽度优先搜索BFS(Breadth-first Search)

> 注意：题目答案在上面内容中都有

- 剑指 Offer 32 - I
- 剑指 Offer 32 - II
- 剑指 Offer 32 - III



## 设计

### 简单难度

[剑指 Offer 09. 用两个栈实现队列 - 力扣（LeetCode）](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

 

示例 1：

输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
示例 2：

输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
提示：

1 <= values <= 10000
最多会对 appendTail、deleteHead 进行 10000 次调用

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
var CQueue = function() {
    this.appendStack = []
    this.deleteStack = [] // 倒序存放的appendStack

};

/** 
 * @param {number} value
 * @return {void}
 */
CQueue.prototype.appendTail = function(value) {
    this.appendStack.push(value)
};

/**
 * @return {number}
 */
CQueue.prototype.deleteHead = function() {
    if(this.deleteStack.length) {
        // this.deleteStack存在元素，直接pop出一个
        return this.deleteStack.pop()
    } else {
        // this.deleteStack为空，直接将this.appendStack放入this.deleteStack
        if(!this.appendStack.length) {
            // 两个栈都为空，返回-1
            return -1
        }
        while(this.appendStack.length) {
            this.deleteStack.push(this.appendStack.pop())
        }
        return this.deleteStack.pop()
    }
};

/**
 * Your CQueue object will be instantiated and called as such:
 * var obj = new CQueue()
 * obj.appendTail(value)
 * var param_2 = obj.deleteHead()
 */
```

[剑指 Offer 30. 包含min函数的栈 - 力扣（LeetCode）](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof/)

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

 

示例:

MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.min();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.min();   --> 返回 -2.
 

提示：

各函数的调用总次数不超过 20000 次


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * initialize your data structure here.
 */
var MinStack = function() {
    this.minVal = null
    this.stack = []
};

/** 
 * @param {number} x
 * @return {void}
 */
MinStack.prototype.push = function(x) {
    this.stack.push(x)
    this.minVal = null // 每次新增元素时重置this.minVal
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function() {
    if(!this.stack.length) {
        return null
    } else {
        this.minVal = null // 每次新增元素时重置this.minVal
        return this.stack.pop()
    }
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
    if(this.stack.length) {
        return this.stack[this.stack.length - 1]
    } else {
        return null
    }
};

/**
 * @return {number}
 */
MinStack.prototype.min = function() {
    if(this.minVal === null) {
        // 需要重新计算，不等于null的时候说明this.stack没有变过，直接返回上一次计算的值
        this.minVal = Number.MAX_SAFE_INTEGER
        for(let i = 0; i < this.stack.length; ++ i) {
            if(this.minVal > this.stack[i]) {
                this.minVal = this.stack[i]
            }
        }
    }
    return this.minVal
};

/**
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(x)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.min()
 */
```

- [剑指 Offer 37. 序列化二叉树 - 力扣（LeetCode）](https://leetcode-cn.com/problems/xu-lie-hua-er-cha-shu-lcof/)
	- 这题参考上文



[剑指 Offer 41. 数据流中的中位数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof/)

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4] 的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

void addNum(int num) - 从数据流中添加一个整数到数据结构中。
double findMedian() - 返回目前所有元素的中位数。
示例 1：

输入：
["MedianFinder","addNum","addNum","findMedian","addNum","findMedian"]
[[],[1],[2],[],[3],[]]
输出：[null,null,null,1.50000,null,2.00000]
示例 2：

输入：
["MedianFinder","addNum","findMedian","addNum","findMedian"]
[[],[2],[],[3],[]]
输出：[null,null,2.00000,null,2.50000]
 

限制：

最多会对 addNum、findMedia进行 50000 次调用。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-ju-liu-zhong-de-zhong-wei-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * initialize your data structure here.
 */
var MedianFinder = function() {
    this.arr = []
};

/** 
 * @param {number} num
 * @return {void}
 */
MedianFinder.prototype.addNum = function(num) {
    this.arr.push(num)
};

/**
 * @return {number}
 */
MedianFinder.prototype.findMedian = function() {
    if(!this.arr.length) {
        return null
    }
    let sortedArr = this.arr.sort((a, b) => a - b)
    if(sortedArr.length % 2 === 0) {
        // 偶数个
        let mid = sortedArr.length / 2
        return (sortedArr[mid - 1] + sortedArr[mid]) / 2
    } else {
        return sortedArr[(sortedArr.length + 1) / 2 - 1]
    }

};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * var obj = new MedianFinder()
 * obj.addNum(num)
 * var param_2 = obj.findMedian()
 */
```


----

## 递归

### 简单难度

[剑指 Offer 10- I. 斐波那契数列 - 力扣（LeetCode）](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof/)

写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项。斐波那契数列的定义如下：

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

 

示例 1：

输入：n = 2
输出：1
示例 2：

输入：n = 5
输出：5
 

提示：

0 <= n <= 100

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number} n
 * @return {number}
 */
let cache = {}
var fib = function(n) {
    let a = 0, b = 1, sum = 0
    for(let i = 0; i < n; ++i) {
        sum = (a + b) % 1000000007 // 注意看题目：这里要按照要求需要取模
        a = b
        b = sum
    }

    return a
};
```

[剑指 Offer 10- II. 青蛙跳台阶问题 - 力扣（LeetCode）](https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof/)

一只青蛙一次可以跳上1级台阶，也可以跳上2级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

示例 1：

输入：n = 2
输出：2
示例 2：

输入：n = 7
输出：21
示例 3：

输入：n = 0
输出：1
提示：

0 <= n <= 100

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/qing-wa-tiao-tai-jie-wen-ti-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number} n
 * @return {number}
 */
var numWays = function(n) {
    let a = 1, b = 1, sum
    for(let i = 0; i < n; ++i) {
        sum = (a + b) % 1000000007;
        a = b;
        b = sum;

        console.log('a, b, sum: ', a, b, sum)
    }

    return a
};
```



### 中等难度

- [剑指 Offer 07. 重建二叉树 - 力扣（LeetCode）](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof/)
	- 参考前文


[剑指 Offer 16. 数值的整数次方 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/)

实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。

 

示例 1:

输入: 2.00000, 10
输出: 1024.00000
示例 2:

输入: 2.10000, 3
输出: 9.26100
示例 3:

输入: 2.00000, -2
输出: 0.25000
解释: 2-2 = 1/22 = 1/4 = 0.25
 

说明:

-100.0 < x < 100.0
n 是 32 位有符号整数，其数值范围是 [−231, 231 − 1] 。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number} x
 * @param {number} n
 * @return {number}
 */

// 题目负n次方的理解参考：一个数的负数次方从本质上讲是怎么计算的？ - lulipro的回答 - 知乎 https://www.zhihu.com/question/267162925/answer/1370184502

// 答案参考：https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof/solution/jian-zhi-offer-16-shu-zhi-de-zheng-shu-ci-fang-d-3/
function pow(x, n) {
    if(n === 0) return 1

    let r = pow(x, parseInt(n / 2))

    if(n%2 === 0) {
        // n为偶数
        return r * r;
    } else {
        return r * r * x;
    }
}

var myPow = function(x, n) {
    if(x === 0) return 0
    if(n >= 0) {
        return pow(x, n)
    } else {
        return 1 / pow(x, n)
    }


};
```

----

## 队列

[剑指 Offer 59 - I. 滑动窗口的最大值 - 力扣（LeetCode）](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)

给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。

示例:

输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 

  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
 

提示：

你可以假设 k 总是有效的，在输入数组不为空的情况下，1 ≤ k ≤ 输入数组的大小。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。



```
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var maxSlidingWindow = function(nums, k) {
    if(!nums.length || !k) return []
    
    let deque = [] // 双向队列
    let res = []

    for(let j = 0, i = 1 - k; j < nums.length; ++i, ++j) {
        if(i > 0 && deque[0] === nums[i - 1]) {
            // 每次删除掉deque左边第一个元素
            deque.shift()
        }        

        while(deque.length && deque[deque.length - 1] < nums[j]) {
            // 从右到左，将deuqe中小于当前nums[j]的元素删除掉
            deque.pop()
        }
        deque.push(nums[j]) // 将本元素压入deque

        if(i >= 0) {
            // 将当前deque中最大的元素放入返回结果中
            res.push(deque[0])
        }
            
    }

    return res
};
```

----

## 数组

### 简单难度


[剑指 Offer 03. 数组中重复的数字 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

示例 1：

输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
 

限制：

2 <= n <= 100000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * @param {number[]} nums
 * @return {number}
 */
var findRepeatNumber = function(nums) {
    let hashMap = {}
    nums.sort((a, b) => a - b)
    for(let i = 0; i < nums.length - 1; ++ i) {
        console.log('nums[i]: ', nums[i])
        if(nums[i] == nums[i + 1]) {
            return nums[i]
        }
    }
};
```

[剑指 Offer 04. 二维数组中的查找 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

 

示例:

现有矩阵 matrix 如下：

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
给定 target = 5，返回 true。

给定 target = 20，返回 false。

 

限制：

0 <= n <= 1000

0 <= m <= 1000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * @param {number[][]} matrix
 * @param {number} target
 * @return {boolean}
 */
var findNumberIn2DArray = function(matrix, target) {
    if(matrix === null || matrix.length === 0 || matrix[0].length === 0) {
        return false
    }
    // 思路：从矩阵的右上角开始遍历
    let rows = matrix.length,
        columns = matrix[0].length
    
    let row = 0,
        column = columns - 1
    
    while(row < rows && column >= 0) {
        let cur = matrix[row][column]
        if(cur === target) {
            return true
        } else if(cur < target) {
            row ++
        } else if(cur > target) {
            column--
        }
    }

    return false
};
```

[剑指 Offer 29. 顺时针打印矩阵 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof/)

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字。

 

示例 1：

输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
示例 2：

输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
 

限制：

0 <= matrix.length <= 100
0 <= matrix[i].length <= 100

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shun-shi-zhen-da-yin-ju-zhen-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * @param {number[][]} matrix
 * @return {number[]}
 */
var spiralOrder = function(matrix) {
    if(matrix === null || !matrix.length || !matrix[0].length) {
        return []
    }

    let rows = matrix.length,
        columns = matrix[0].length
    
    let visited = [] // 访问记录矩阵
    for(let i = 0; i < matrix.length; ++ i) {
        visited.push([])
    }

    let res = [] // 返回结果
    let row = 0, column = 0
    let directions = [
        [0, 1],
        [1, 0],
        [0, -1],
        [-1, 0]
    ]
    let directionIndex = 0
    let total = rows * columns

    for(let i = 0; i < total; ++ i) {
        res.push(matrix[row][column])
        visited[row][column] = true
        let nextRow = row + directions[directionIndex][0],
            nextColumn = column + directions[directionIndex][1]
        if(nextRow < 0 || nextRow >= rows || nextColumn < 0 || nextColumn >= columns || visited[nextRow][nextColumn]) {
            directionIndex = (directionIndex + 1) % 4
        }

        row += directions[directionIndex][0]
        column += directions[directionIndex][1]
    }

    return res


};
```

[剑指 Offer 53 - I. 在排序数组中查找数字 I - 力扣（LeetCode）](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

统计一个数字在排序数组中出现的次数。

 

示例 1:

输入: nums = [5,7,7,8,8,10], target = 8
输出: 2
示例 2:

输入: nums = [5,7,7,8,8,10], target = 6
输出: 0
 

限制：

0 <= 数组长度 <= 50000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
function helper(nums, target) {
    let i = 0,
        j = nums.length - 1
    
    while(i <= j) {
        let mid = parseInt((i + j) / 2)
        if(nums[mid] <= target) {
            i = mid + 1
        } else {
            j = mid - 1
        }
    }

    return i
}

var search = function(nums, target) {
    return helper(nums, target) - helper(nums, target - 1)
};
```

[剑指 Offer 53 - II. 0～n-1中缺失的数字 - 力扣（LeetCode）](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)

一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

 

示例 1:

输入: [0,1,3]
输出: 2
示例 2:

输入: [0,1,2,3,4,5,6,7,9]
输出: 8
 

限制：

1 <= 数组长度 <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number[]} nums
 * @return {number}
 */
var missingNumber = function(nums) {
    let left = 0,
    right = nums.length - 1;
    while(left <= right) {
        let mid = Math.floor((left + right) / 2);
        if(mid === nums[mid]) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }

    return left;
    
};

```


----

## 哈希表

[剑指 Offer 03. 数组中重复的数字 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

找出数组中重复的数字。


在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

示例 1：

输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
 

限制：

2 <= n <= 100000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number[]} nums
 * @return {number}
 */
var findRepeatNumber = function(nums) {
    let hashMap = {}
    nums.sort((a, b) => a - b)
    for(let i = 0; i < nums.length - 1; ++ i) {
        console.log('nums[i]: ', nums[i])
        if(nums[i] == nums[i + 1]) {
            return nums[i]
        }
    }
};
```

[剑指 Offer 48. 最长不含重复字符的子字符串 - 力扣（LeetCode）](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

 

示例 1:

输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
示例 2:

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
示例 3:

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
 

提示：

s.length <= 40000


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    let dic = new Map()

    let i = -1,
        res = 0
    
    for(let j = 0; j < s.length; ++j) {
        if(dic.has(s.charAt(j))) {
            i = Math.max(i, dic.get(s.charAt(j)))
        }
        dic.set(s.charAt(j), j) // 设置当前字符及其index
        res = Math.max(res, j - i) // 统计当前最长字符串
    }

    return res
};
```


[剑指 Offer 50. 第一个只出现一次的字符 - 力扣（LeetCode）](https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof/)

在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

示例:

s = "abaccdeff"
返回 "b"

s = "" 
返回 " "
 

限制：

0 <= s 的长度 <= 50000



来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/di-yi-ge-zhi-chu-xian-yi-ci-de-zi-fu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {string} s
 * @return {character}
 */
var firstUniqChar = function(s) {
    if(!s) {
        return ' '
    }
    let hashMap = new Map()
    let sArr = s.split('')

    for(let i = 0; i < sArr.length; ++ i) {
        let item = sArr[i]
        if(hashMap.get(item) === undefined) {
            hashMap.set(item, true)
        } else if(hashMap.get(item)) {
            hashMap.set(item, false)
        }
    }

    for(let [key, value] of hashMap) {
        if(value) {
            return key
        }
    }

    return ' '
    
};
```

----

## 链表

[剑指 Offer 06. 从尾到头打印链表 - 力扣（LeetCode）](https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof/)

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）。

 

示例 1：

输入：head = [1,3,2]
输出：[2,3,1]
 

限制：

0 <= 链表长度 <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/cong-wei-dao-tou-da-yin-lian-biao-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {number[]}
 */
var reversePrint = function(head) {
    let stack = []
    let res = []

    while(head !== null) {
        stack.push(head.val)
        head = head.next
    }

    while(stack.length) {
        res.push(stack.pop())
    }

    return res
};
```

[剑指 Offer 18. 删除链表的节点 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof/)

给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

注意：此题对比原题有改动

示例 1:

输入: head = [4,5,1,9], val = 5
输出: [4,1,9]
解释: 给定你链表中值为 5 的第二个节点，那么在调用了你的函数之后，该链表应变为 4 -> 1 -> 9.
示例 2:

输入: head = [4,5,1,9], val = 1
输出: [4,5,9]
解释: 给定你链表中值为 1 的第三个节点，那么在调用了你的函数之后，该链表应变为 4 -> 5 -> 9.
 

说明：

题目保证链表中节点的值互不相同
若使用 C 或 C++ 语言，你不需要 free 或 delete 被删除的节点

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shan-chu-lian-biao-de-jie-dian-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} val
 * @return {ListNode}
 */
var deleteNode = function(head, val) {
    let pre = head,
        cur = head
    
    if(cur.val === val) {
        return cur.next
    }

    while(cur && cur.next) {
        if(cur.next.val === val) {
            cur.next = cur.next.next
        } else {
            cur = cur.next
        }
    }

    return pre
};
```

[剑指 Offer 22. 链表中倒数第k个节点 - 力扣（LeetCode）](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。

 

示例：

给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} k
 * @return {ListNode}
 */
var getKthFromEnd = function(head, k) {
    let former = head,
        latter = head
    
    for(let i = 0; i < k; ++i) {
        former = former.next
    }

    while(former !== null) {
        former = former.next
        latter = latter.next
    }
    
    return latter
    
};
```

[剑指 Offer 24. 反转链表 - 力扣（LeetCode）](https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof/)

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

 

示例:

输入: 1->2->3->4->5->NULL
输出: 5->4->3->2->1->NULL
 

限制：

0 <= 节点个数 <= 5000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/fan-zhuan-lian-biao-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function(head) {
    let cur = head,
        pre = null,
        temp = null

    while(cur) {
        temp = cur.next
        cur.next = pre
        pre = cur
        cur = temp
    }

    return pre


};
```

[剑指 Offer 52. 两个链表的第一个公共节点 - 力扣（LeetCode）](https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof/)

输入两个链表，找出它们的第一个公共节点。

如下面的两个链表：



在节点 c1 开始相交。

 

示例 1：



输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3
输出：Reference of the node with value = 8
输入解释：相交节点的值为 8 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,0,1,8,4,5]。在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
 

示例 2：



输入：intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Reference of the node with value = 2
输入解释：相交节点的值为 2 （注意，如果两个列表相交则不能为 0）。从各自的表头开始算起，链表 A 为 [0,9,1,2,4]，链表 B 为 [3,2,4]。在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
 

示例 3：



输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
输入解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
解释：这两个链表不相交，因此返回 null。
 

注意：

如果两个链表没有交点，返回 null.
在返回结果后，两个链表仍须保持原有的结构。
可假定整个链表结构中没有循环。
程序尽量满足 O(n) 时间复杂度，且仅用 O(1) 内存。
本题与主站 160 题相同：https://leetcode-cn.com/problems/intersection-of-two-linked-lists/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/liang-ge-lian-biao-de-di-yi-ge-gong-gong-jie-dian-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */

/**
 * @param {ListNode} headA
 * @param {ListNode} headB
 * @return {ListNode}
 */
var getIntersectionNode = function(headA, headB) {
    let nodeA = headA,
        nodeB = headB
    
    while(nodeA !== nodeB) {
        nodeA = nodeA === null ? headA : nodeA.next
        nodeB = nodeB === null ? headB : nodeB.next
    }

    return nodeA
};
```

----

## 数学-基本为中等

[剑指 Offer 14- I. 剪绳子 - 力扣（LeetCode）](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m-1] 。请问 k[0]*k[1]*...*k[m-1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

示例 1：

输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
示例 2:

输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
提示：

2 <= n <= 58
注意：本题与主站 343 题相同：https://leetcode-cn.com/problems/integer-break/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/jian-sheng-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number} n
 * @return {number}
 */
var cuttingRope = function(n) {
    if(n <= 3) {
        // 只能是[1,2]或者[1,1]
        return n - 1
    }

    let a = parseInt(n / 3),
        b = n % 3
    
    if(b === 0) {
        return Math.pow(3, a)
    } else if(b === 1) {
        return Math.pow(3, a - 1) * 4 // 4 = 3 + 余数1
    } else {
        return Math.pow(3, a) * 2
    }
};
```

[剑指 Offer 14- II. 剪绳子 II - 力扣（LeetCode）](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/)

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m - 1] 。请问 k[0]*k[1]*...*k[m - 1] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

 

示例 1：

输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
示例 2:

输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36
 

提示：

2 <= n <= 1000
注意：本题与主站 343 题相同：https://leetcode-cn.com/problems/integer-break/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * @param {number} n
 * @return {number}
 */
var cuttingRope = function(n) {
    if(n <= 3) {
        // 只能是[1,2]或者[1,1]
        return (n - 1) % 1000000007
    }

    let a = parseInt(n / 3),
        b = n % 3
    
    let res = 1
    let i = 0
    const max=1e9+7

    while(i < a - 1) {
        res = (res%max) * 3
        ++i
    }
    
    if(b === 0) {
        res =  (res%max) * 3
    } else if(b === 1) {
        console.log('b === 1')
        res = (res%max) * 4 // 4 = 3 + 余数1
    } else {
        res = (res%max) * 3 * 2
    }

    return res % 1000000007
};
```

[剑指 Offer 17. 打印从1到最大的n位数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof/)

输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999。

示例 1:

输入: n = 1
输出: [1,2,3,4,5,6,7,8,9]
 

说明：

用返回一个整数列表来代替打印
n 为正整数

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/da-yin-cong-1dao-zui-da-de-nwei-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number} n
 * @return {number[]}
 */
var printNumbers = function(n) {
    let res = []
    let max = 0
    let i = 0
    while(i < n) {
        max = max * 10 + 9
        ++i
    }

    for(let j = 1; j <= max; ++j) {
        res.push(j)
    }

    return res
};
```


[剑指 Offer 43. 1～n整数中1出现的次数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof/)

输入一个整数 n ，求1～n这n个整数的十进制表示中1出现的次数。

例如，输入12，1～12这些整数中包含1 的数字有1、10、11和12，1一共出现了5次。

 

示例 1：

输入：n = 12
输出：5
示例 2：

输入：n = 13
输出：6
 

限制：

1 <= n < 2^31
注意：本题与主站 233 题相同：https://leetcode-cn.com/problems/number-of-digit-one/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/1nzheng-shu-zhong-1chu-xian-de-ci-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number} n
 * @return {number}
 */
// 思路参考： [剑指offer-整数中1出现的次数（从1到n整数中1出现的次数）_Genge -CSDN博客](https://blog.csdn.net/huzhigenlaohu/article/details/51779365)
var countDigitOne = function(n) {
    let digit = 1,
        res = 0
    
    let high = parseInt(n / 10),
        cur = n % 10,
        low = 0
    
    while(high !== 0 || cur !== 0) {
        if(cur === 0) {
            // 当前位cur为0，出现1的次数由high决定
            res += high * digit
        } else if(cur === 1) {
            // 当前位cur为1、low位也决定cur出现的次数，所以需要加上low
            res += high * digit + low + 1
        } else {
            // 当前位cur > 1，相当于进位。例如：13与20在个位数出现1的次数是一样的
            res += (high + 1) * digit
        }
        low += cur * digit // 低位区
        cur = high % 10; // 当前位
        high = parseInt(high / 10) // 高位
        digit *= 10; // cur位对应的位数。例如：100，cur = 1的时候，digit = 100
    }

    return res
};
```

[剑指 Offer 67. 把字符串转换成整数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)

写一个函数 StrToInt，实现把字符串转换成整数这个功能。不能使用 atoi 或者其他类似的库函数。

 

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0。

说明：

假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

示例 1:

输入: "42"
输出: 42
示例 2:

输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
示例 3:

输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
示例 4:

输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
示例 5:

输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。
 

注意：本题与主站 8 题相同：https://leetcode-cn.com/problems/string-to-integer-atoi/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * @param {string} str
 * @return {number}
 */
var strToInt = function(str) {
    let strArr = str.trim().split('') // 注意：直接使用.split()不会分割字符串

    if(!strArr.length) return 0

    let res = 0,
        border = parseInt(2147483647 / 10) // 整数边界：2的31次方减1
    let i = 1, // 数字起始index
        sign = 1 // 正负标志
    
    if(strArr[0] === '-') {
        // 负数
        sign = -1
    } else if(strArr[0] !== '+') {
        // 不是以"+\-"号开始，将index置为0
        i = 0
    }

    for(let j = i; j < strArr.length; ++j) {
        if(strArr[j] < '0' || strArr[j] > '9') break
        if(res > border || res === border && strArr[j] > '7') {
            return sign === 1 ? 2147483647 : -2147483648
        }
        res = res * 10 + parseInt(strArr[j])
    }

    return res * sign
};
```

----

## 双指针

[剑指 Offer 04. 二维数组中的查找 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof/)

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

 

示例:

现有矩阵 matrix 如下：

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
给定 target = 5，返回 true。

给定 target = 20，返回 false。

 

限制：

0 <= n <= 1000

0 <= m <= 1000

 

注意：本题与主站 240 题相同：https://leetcode-cn.com/problems/search-a-2d-matrix-ii/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-wei-shu-zu-zhong-de-cha-zhao-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number[][]} matrix
 * @param {number} target
 * @return {boolean}
 */
var findNumberIn2DArray = function(matrix, target) {
    if(matrix === null || matrix.length === 0 || matrix[0].length === 0) {
        return false
    }
    // 思路：从矩阵的右上角开始遍历
    let rows = matrix.length,
        columns = matrix[0].length
    
    let row = 0,
        column = columns - 1
    
    while(row < rows && column >= 0) {
        let cur = matrix[row][column]
        if(cur === target) {
            return true
        } else if(cur < target) {
            row ++
        } else if(cur > target) {
            column--
        }
    }

    return false
};
```

[剑指 Offer 22. 链表中倒数第k个节点 - 力扣（LeetCode）](https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof/)

输入一个链表，输出该链表中倒数第k个节点。为了符合大多数人的习惯，本题从1开始计数，即链表的尾节点是倒数第1个节点。例如，一个链表有6个节点，从头节点开始，它们的值依次是1、2、3、4、5、6。这个链表的倒数第3个节点是值为4的节点。

 

示例：

给定一个链表: 1->2->3->4->5, 和 k = 2.

返回链表 4->5.

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/lian-biao-zhong-dao-shu-di-kge-jie-dian-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @param {number} k
 * @return {ListNode}
 */
var getKthFromEnd = function(head, k) {
    let former = head,
        latter = head
    
    for(let i = 0; i < k; ++i) {
        former = former.next
    }

    while(former !== null) {
        former = former.next
        latter = latter.next
    }
    
    return latter
    
};
```


- [剑指 Offer 48. 最长不含重复字符的子字符串 - 力扣（LeetCode）](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)
	- 参考上文


----

## 字符串

[剑指 Offer 58 - I. 翻转单词顺序 - 力扣（LeetCode）](https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof/)

输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

 

示例 1：

输入: "the sky is blue"
输出: "blue is sky the"
示例 2：

输入: "  hello world!  "
输出: "world! hello"
解释: 输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
示例 3：

输入: "a good   example"
输出: "example good a"
解释: 如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
 

说明：

无空格字符构成一个单词。
输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。
如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。
注意：本题与主站 151 题相同：https://leetcode-cn.com/problems/reverse-words-in-a-string/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/fan-zhuan-dan-ci-shun-xu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * @param {string} s
 * @return {string}
 */
var reverseWords = function(s) {
    s = s.trim()
    let right = s.length- 1,
        left = right
    
    let res = ''

    while(left >= 0 ) {
        while(left >= 0 && s.charAt(left) != ' ') {
            --left
        }
        res += s.substring(left + 1, right + 1) + ' '
        while(left >= 0 && s.charAt(left) === ' ') {
            -- left
        }
        right = left
    }
    
    return res.trim()
};
```

[剑指 Offer 58 - II. 左旋转字符串 - 力扣（LeetCode）](https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof/)

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字2，该函数将返回左旋转两位得到的结果"cdefgab"。

 

示例 1：

输入: s = "abcdefg", k = 2
输出: "cdefgab"
示例 2：

输入: s = "lrloseumgh", k = 6
输出: "umghlrlose"
 

限制：

1 <= k < s.length <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zuo-xuan-zhuan-zi-fu-chuan-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {string} s
 * @param {number} n
 * @return {string}
 */
var reverseLeftWords = function(s, n) {
    return s.substring(n, s.length) + s.substring(0, n)
};
```

[剑指 Offer 67. 把字符串转换成整数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof/)

写一个函数 StrToInt，实现把字符串转换成整数这个功能。不能使用 atoi 或者其他类似的库函数。

 

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0。

说明：

假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−231,  231 − 1]。如果数值超过这个范围，请返回  INT_MAX (231 − 1) 或 INT_MIN (−231) 。

示例 1:

输入: "42"
输出: 42
示例 2:

输入: "   -42"
输出: -42
解释: 第一个非空白字符为 '-', 它是一个负号。
     我们尽可能将负号与后面所有连续出现的数字组合起来，最后得到 -42 。
示例 3:

输入: "4193 with words"
输出: 4193
解释: 转换截止于数字 '3' ，因为它的下一个字符不为数字。
示例 4:

输入: "words and 987"
输出: 0
解释: 第一个非空字符是 'w', 但它不是数字或正、负号。
     因此无法执行有效的转换。
示例 5:

输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 
     因此返回 INT_MIN (−231) 。
 

注意：本题与主站 8 题相同：https://leetcode-cn.com/problems/string-to-integer-atoi/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ba-zi-fu-chuan-zhuan-huan-cheng-zheng-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {string} str
 * @return {number}
 */
var strToInt = function(str) {
    let strArr = str.trim().split('') // 注意：直接使用.split()不会分割字符串

    if(!strArr.length) return 0

    let res = 0,
        border = parseInt(2147483647 / 10) // 整数边界：2的31次方减1
    let i = 1, // 数字起始index
        sign = 1 // 正负标志
    
    if(strArr[0] === '-') {
        // 负数
        sign = -1
    } else if(strArr[0] !== '+') {
        // 不是以"+\-"号开始，将index置为0
        i = 0
    }

    for(let j = i; j < strArr.length; ++j) {
        if(strArr[j] < '0' || strArr[j] > '9') break
        if(res > border || res === border && strArr[j] > '7') {
            return sign === 1 ? 2147483647 : -2147483648
        }
        res = res * 10 + parseInt(strArr[j])
    }

    return res * sign
};
```

----

## 二分查找

[剑指 Offer 11. 旋转数组的最小数字 - 力扣（LeetCode）](https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof/)

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为1。  

示例 1：

输入：[3,4,5,1,2]
输出：1
示例 2：

输入：[2,2,2,0,1]
输出：0
注意：本题与主站 154 题相同：https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/xuan-zhuan-shu-zu-de-zui-xiao-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * @param {number[]} numbers
 * @return {number}
 */
var minArray = function(numbers) {
    let low = 0
    let high = numbers.length - 1

    while(low < high) {
        let pivot = low + parseInt((high - low) / 2)
        if(numbers[pivot] < numbers[high]) {
            high = pivot
        } else if(numbers[pivot] > numbers[high]) {
            low = pivot + 1
        } else {
            // numbers[pivot] == numbers[high]，左移high，直到high和pivot不相等
            -- high
        }
    }

    return numbers[low]
};
```

[剑指 Offer 53 - I. 在排序数组中查找数字 I - 力扣（LeetCode）](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

统计一个数字在排序数组中出现的次数。

 

示例 1:

输入: nums = [5,7,7,8,8,10], target = 8
输出: 2
示例 2:

输入: nums = [5,7,7,8,8,10], target = 6
输出: 0
 

限制：

0 <= 数组长度 <= 50000

 

注意：本题与主站 34 题相同（仅返回值不同）：https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number}
 */
function helper(nums, target) {
    let i = 0,
        j = nums.length - 1
    
    while(i <= j) {
        let mid = parseInt((i + j) / 2)
        if(nums[mid] <= target) {
            i = mid + 1
        } else {
            j = mid - 1
        }
    }

    return i
}

var search = function(nums, target) {
    return helper(nums, target) - helper(nums, target - 1)
};
```

[剑指 Offer 53 - II. 0～n-1中缺失的数字 - 力扣（LeetCode）](https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof/)
一个长度为n-1的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围0～n-1之内。在范围0～n-1内的n个数字中有且只有一个数字不在该数组中，请找出这个数字。

 

示例 1:

输入: [0,1,3]
输出: 2
示例 2:

输入: [0,1,2,3,4,5,6,7,9]
输出: 8
 

限制：

1 <= 数组长度 <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/que-shi-de-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number[]} nums
 * @return {number}
 */
var missingNumber = function(nums) {
    let left = 0,
    right = nums.length - 1;
    while(left <= right) {
        let mid = Math.floor((left + right) / 2);
        if(mid === nums[mid]) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }

    return left;
    
};

```

----

## 分治算法

[剑指 Offer 25. 合并两个排序的链表 - 力扣（LeetCode）](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

示例1：

输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4
限制：

0 <= 链表长度 <= 1000

注意：本题与主站 21 题相同：https://leetcode-cn.com/problems/merge-two-sorted-lists/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function(l1, l2) {
    let head = new ListNode(),
        cur = head


    while(l1 !== null && l2 !== null) {
        if(l1.val > l2.val) {
            cur.next = l2
            cur = l2
            l2 = l2.next
        } else {
            cur.next = l1
            cur = l1
            l1 = l1.next
        }
    }

    if(l1 !== null) {
        while(l1 !== null) {
            cur.next = l1
            cur = l1
            l1 = l1.next
        }
    }

    if(l2 !== null) {
        while(l2 !== null) {
            cur.next = l2
            cur = l2
            l2 = l2.next
        }
    }

    return head.next
};
```

[剑指 Offer 36. 二叉搜索树与双向链表 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的循环双向链表。要求不能创建任何新的节点，只能调整树中节点指针的指向。

 

为了让您更好地理解问题，以下面的二叉搜索树为例：

 



 

我们希望将这个二叉搜索树转化为双向循环链表。链表中的每个节点都有一个前驱和后继指针。对于双向循环链表，第一个节点的前驱是最后一个节点，最后一个节点的后继是第一个节点。

下图展示了上面的二叉搜索树转化成的链表。“head” 表示指向链表中有最小元素的节点。

 



 

特别地，我们希望可以就地完成转换操作。当转化完成以后，树中节点的左指针需要指向前驱，树中节点的右指针需要指向后继。还需要返回链表中的第一个节点的指针。

 

注意：本题与主站 426 题相同：https://leetcode-cn.com/problems/convert-binary-search-tree-to-sorted-doubly-linked-list/

注意：此题对比原题有改动。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * // Definition for a Node.
 * function Node(val,left,right) {
 *    this.val = val;
 *    this.left = left;
 *    this.right = right;
 * };
 */
/**
 * @param {Node} root
 * @return {Node}
 */
var treeToDoublyList = function(root) {
    let pre = null,
        head = null
    
    function dfs(node) {
        if(node == null) return
        dfs(node.left)
        if(pre !== null) {
            pre.right = node
        } else {
            head = node
        }
        node.left = pre
        pre = node
        dfs(node.right)
    }

    if(root === null) return null
    dfs(root)
    head.left = pre
    pre.right = head

    return head
};
```

[剑指 Offer 39. 数组中出现次数超过一半的数字 - 力扣（LeetCode）](https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof/)

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。

 

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

 

示例 1:

输入: [1, 2, 3, 2, 2, 2, 5, 4, 2]
输出: 2
 

限制：

1 <= 数组长度 <= 50000

 

注意：本题与主站 169 题相同：https://leetcode-cn.com/problems/majority-element/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/shu-zu-zhong-chu-xian-ci-shu-chao-guo-yi-ban-de-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number[]} nums
 * @return {number}
 */
// 使用正负抵消法
var majorityElement = function(nums) {
    let currentNum = null,
        numCounter = 0;
    for(let i = 0; i < nums.length; ++ i) {
        if(numCounter === 0) {
            currentNum = nums[i]
        }
        numCounter += currentNum === nums[i] ? 1 : -1
    }

    return currentNum
};
```

[剑指 Offer 40. 最小的k个数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof/)

输入整数数组 arr ，找出其中最小的 k 个数。例如，输入4、5、1、6、2、7、3、8这8个数字，则最小的4个数字是1、2、3、4。

 

示例 1：

输入：arr = [3,2,1], k = 2
输出：[1,2] 或者 [2,1]
示例 2：

输入：arr = [0,1,2,1], k = 1
输出：[0]
 

限制：

0 <= k <= arr.length <= 10000
0 <= arr[i] <= 10000


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zui-xiao-de-kge-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number[]} arr
 * @param {number} k
 * @return {number[]}
 */
var getLeastNumbers = function(arr, k) {
    return arr.sort((a, b) => a - b).slice(0, k)
};
```

[剑指 Offer 42. 连续子数组的最大和 - 力扣（LeetCode）](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

 

示例1:

输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
 

提示：

1 <= arr.length <= 10^5
-100 <= arr[i] <= 100
注意：本题与主站 53 题相同：https://leetcode-cn.com/problems/maximum-subarray/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
    if(!nums.length) return 0
    let res = nums[0]

    for(let i = 1; i < nums.length; ++i) {
        nums[i] += Math.max(nums[i - 1], 0) // 借用nums[i]的空间存储；小于0则加0
        res = Math.max(res, nums[i])
    }

    return res
};
```

----

## 动态规划

- [剑指 Offer 14- I. 剪绳子 - 力扣（LeetCode）](https://leetcode-cn.com/problems/jian-sheng-zi-lcof/)
- [剑指 Offer 14- II. 剪绳子 II - 力扣（LeetCode）](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/)
	- 上面两题参考上文

[剑指 Offer 42. 连续子数组的最大和 - 力扣（LeetCode）](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)

输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。

要求时间复杂度为O(n)。

 

示例1:

输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
输出: 6
解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
 

提示：

1 <= arr.length <= 10^5
-100 <= arr[i] <= 100
注意：本题与主站 53 题相同：https://leetcode-cn.com/problems/maximum-subarray/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
    if(!nums.length) return 0
    let res = nums[0]

    for(let i = 1; i < nums.length; ++i) {
        nums[i] += Math.max(nums[i - 1], 0) // 借用nums[i]的空间存储；小于0则加0
        res = Math.max(res, nums[i])
    }

    return res
};
```

[剑指 Offer 47. 礼物的最大价值 - 力扣（LeetCode）](https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof/)

在一个 m*n 的棋盘的每一格都放有一个礼物，每个礼物都有一定的价值（价值大于 0）。你可以从棋盘的左上角开始拿格子里的礼物，并每次向右或者向下移动一格、直到到达棋盘的右下角。给定一个棋盘及其上面的礼物的价值，请计算你最多能拿到多少价值的礼物？

 

示例 1:

输入: 
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
输出: 12
解释: 路径 1→3→5→2→1 可以拿到最多价值的礼物
 

提示：

0 < grid.length <= 200
0 < grid[0].length <= 200


来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/li-wu-de-zui-da-jie-zhi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * @param {number[][]} grid
 * @return {number}
 */
var maxValue = function(grid) {
    let m = grid.length,
        n = grid[0].length
    
    for(let i = 0; i < m; ++i) {
        for(let j = 0; j < n; ++j) {
            if(i == 0 && j == 0) continue
            if(i == 0) {
                grid[i][j] += grid[i][j - 1]
            } else if(j == 0) {
                grid[i][j] += grid[i - 1][j]
            } else {
                grid[i][j] += Math.max(grid[i][j - 1], grid[i - 1][j])
            }
        }
    }

    return grid[m - 1][n - 1]
};
```

[剑指 Offer 63. 股票的最大利润 - 力扣（LeetCode）](https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof/)

假设把某股票的价格按照时间先后顺序存储在数组中，请问买卖该股票一次可能获得的最大利润是多少？

 

示例 1:

输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
示例 2:

输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
 

限制：

0 <= 数组长度 <= 10^5

 

注意：本题与主站 121 题相同：https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/gu-piao-de-zui-da-li-run-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number[]} prices
 * @return {number}
 */
var maxProfit = function(prices) {
    let cost = Number.MAX_VALUE,
        profit = 0
    
    for(let i = 0; i < prices.length; ++i) {
        cost = Math.min(cost, prices[i])
        profit = Math.max(profit, prices[i] - cost)
    }

    return profit
};
```

----

## 回溯算法

- [剑指 Offer 38. 字符串的排列 - 力扣（LeetCode）](https://leetcode-cn.com/problems/zi-fu-chuan-de-pai-lie-lcof/)

----

## 滑动窗口

[剑指 Offer 48. 最长不含重复字符的子字符串 - 力扣（LeetCode）](https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof/)

请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

 

示例 1:

输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
示例 2:

输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
示例 3:

输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
 

提示：

s.length <= 40000
注意：本题与主站 3 题相同：https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zui-chang-bu-han-zhong-fu-zi-fu-de-zi-zi-fu-chuan-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {string} s
 * @return {number}
 */
var lengthOfLongestSubstring = function(s) {
    let dic = new Map()

    let i = -1,
        res = 0
    
    for(let j = 0; j < s.length; ++j) {
        if(dic.has(s.charAt(j))) {
            i = Math.max(i, dic.get(s.charAt(j)))
        }
        dic.set(s.charAt(j), j) // 设置当前字符及其index
        res = Math.max(res, j - i) // 统计当前最长字符串
    }

    return res
};
```

- [剑指 Offer 59 - I. 滑动窗口的最大值 - 力扣（LeetCode）](https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof/)
	- 参考上文


[剑指 Offer 59 - II. 队列的最大值 - 力扣（LeetCode）](https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof/)

请定义一个队列并实现函数 max_value 得到队列里的最大值，要求函数max_value、push_back 和 pop_front 的均摊时间复杂度都是O(1)。

若队列为空，pop_front 和 max_value 需要返回 -1

示例 1：

输入: 
["MaxQueue","push_back","push_back","max_value","pop_front","max_value"]
[[],[1],[2],[],[],[]]
输出: [null,null,null,2,1,2]
示例 2：

输入: 
["MaxQueue","pop_front","max_value"]
[[],[],[]]
输出: [null,-1,-1]
 

限制：

1 <= push_back,pop_front,max_value的总操作数 <= 10000
1 <= value <= 10^5

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/dui-lie-de-zui-da-zhi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
var MaxQueue = function() {
    this.queue = []
};

/**
 * @return {number}
 */
MaxQueue.prototype.max_value = function() {
    if(!this.queue.length) {
        return -1
    }
    let tempMax = this.queue[0]
    for(let i = 1; i < this.queue.length; ++ i) {
        if(this.queue[i] > tempMax) {
            tempMax = this.queue[i]
        }
    }
    return tempMax
};

/** 
 * @param {number} value
 * @return {void}
 */
MaxQueue.prototype.push_back = function(value) {
    this.queue.push(value)
};

/**
 * @return {number}
 */
MaxQueue.prototype.pop_front = function() {
    if(!this.queue.length) {
        return -1
    }
    return this.queue.shift()
};

/**
 * Your MaxQueue object will be instantiated and called as such:
 * var obj = new MaxQueue()
 * var param_1 = obj.max_value()
 * obj.push_back(value)
 * var param_3 = obj.pop_front()
 */
```


----

## 其他未分类的题目

[剑指 Offer 57 - II. 和为s的连续正数序列 - 力扣（LeetCode）](https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)

输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

 

示例 1：

输入：target = 9
输出：[[2,3,4],[4,5]]
示例 2：

输入：target = 15
输出：[[1,2,3,4,5],[4,5,6],[7,8]]
 

限制：

1 <= target <= 10^5

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * @param {number} target
 * @return {number[][]}
 */
var findContinuousSequence = function(target) {
    let i = 1,
        j = 1,
        sum = 0
    let res = []
    let mid = target / 2 // 滑动窗口中，中间位置的值不会大于taget/2

    while(i < mid) {
        if(sum < target) {
            sum += j
            ++j
        } else if(sum > target) {
            sum -= i
            ++i
        } else {
            // match到结果
            let arr = []
            for(let key = i; key < j; ++key) {
                arr.push(key)
            }
            res.push(arr)
            sum -= i
            ++i
        }
    }

    return res
};
```


[剑指 Offer 13. 机器人的运动范围 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof/)

地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

 

示例 1：

输入：m = 2, n = 3, k = 1
输出：3
示例 2：

输入：m = 3, n = 1, k = 0
输出：1
提示：

1 <= n,m <= 100
0 <= k <= 20

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ji-qi-ren-de-yun-dong-fan-wei-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number} m
 * @param {number} n
 * @param {number} k
 * @return {number}
 */
function digitSum(digit) {
    let sum = 0
    while(digit > 0) {
        sum += digit % 10
        digit = parseInt(digit / 10)
    }

    return sum
}


var movingCount = function(m, n, k) {
    if(!k) return 1
    let visited = [] // 访问过的

    for(let i = 0; i < m; ++i) {
        visited.push([])
    }

    function dfs(m, n, row, col, k) {
        let sum = digitSum(row) + digitSum(col)
        if(row >= m || col >= n || sum > k || visited[row][col]) {
            return 0
        }

        visited[row][col] = true

        return 1 + dfs(m, n, row + 1, col, k) + dfs(m, n, row, col + 1, k)
    }

    return dfs(m, n, 0, 0, k)

    




};
```

[剑指 Offer 05. 替换空格 - 力扣（LeetCode）](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof/)

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

 

示例 1：

输入：s = "We are happy."
输出："We%20are%20happy."
 

限制：

0 <= s 的长度 <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {string} s
 * @return {string}
 */
var replaceSpace = function(s) {
    let newArr = []

    for(let i = 0; i < s.length; ++i) {
        if(s.charAt(i) !== ' ') {
            newArr.push(s.charAt(i))
        } else {
            newArr.push('%20')
        }
    }

    return newArr.join('')
};
```

[剑指 Offer 64. 求1+2+…+n - 力扣（LeetCode）](https://leetcode-cn.com/problems/qiu-12n-lcof/)

求 1+2+...+n ，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

 

示例 1：

输入: n = 3
输出: 6
示例 2：

输入: n = 9
输出: 45
 

限制：

1 <= n <= 10000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/qiu-12n-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number} n
 * @return {number}
 */
var sumNums = function(n) {
    return n && (n + sumNums(n - 1))
};
```

[剑指 Offer 15. 二进制中1的个数 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof/)

请实现一个函数，输入一个整数，输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。

示例 1：

输入：00000000000000000000000000001011
输出：3
解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
示例 2：

输入：00000000000000000000000010000000
输出：1
解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。
示例 3：

输入：11111111111111111111111111111101
输出：31
解释：输入的二进制串 11111111111111111111111111111101 中，共有 31 位为 '1'。
 

注意：本题与主站 191 题相同：https://leetcode-cn.com/problems/number-of-1-bits/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number} n - a positive integer
 * @return {number}
 */
var hammingWeight = function(n) {
    let i =0;

    while(n){
        n = n&(n-1);
        i++;
    }

    return i;
};
```

[剑指 Offer 62. 圆圈中最后剩下的数字 - 力扣（LeetCode）](https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)

0,1,,n-1这n个数字排成一个圆圈，从数字0开始，每次从这个圆圈里删除第m个数字。求出这个圆圈里剩下的最后一个数字。

例如，0、1、2、3、4这5个数字组成一个圆圈，从数字0开始每次删除第3个数字，则删除的前4个数字依次是2、0、4、1，因此最后剩下的数字是3。

 

示例 1：

输入: n = 5, m = 3
输出: 3
示例 2：

输入: n = 10, m = 17
输出: 2
 

限制：

1 <= n <= 10^5
1 <= m <= 10^6

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
/**
 * @param {number} n
 * @param {number} m
 * @return {number}
 */

// function cur(n, m) {
//     if(n === 1) {
//         return 0
//     }
//     let x = cur(n - 1, m)

//     return (m + x) % n
// }

var lastRemaining = function(n, m) {
    let ans = 0
    for(let i = 2; i <= n; ++i) {
        ans = (ans + m) % i
    }

    return ans
    // return cur(n, m)
};
```

[剑指 Offer 61. 扑克牌中的顺子 - 力扣（LeetCode）](https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof/)

从扑克牌中随机抽5张牌，判断是不是一个顺子，即这5张牌是不是连续的。2～10为数字本身，A为1，J为11，Q为12，K为13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。

 

示例 1:

输入: [1,2,3,4,5]
输出: True
 

示例 2:

输入: [0,0,1,2,5]
输出: True
 

限制：

数组长度为 5 

数组的数取值为 [0, 13] .

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/bu-ke-pai-zhong-de-shun-zi-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。

```
/**
 * @param {number[]} nums
 * @return {boolean}
 */
var isStraight = function(nums) {
    let repeatMap = {}
    let max = 0,
        min = 14 // 扑克牌中最大为13，所以设置为14
    for(let i = 0; i < nums.length; ++i) {
        let num = nums[i]
        if(num === 0) continue
        max = Math.max(max, num)
        min = Math.min(min, num)
        if(repeatMap[num]) return false
        repeatMap[num] = true
    }

    return max - min < 5

};
```

[剑指 Offer 33. 二叉搜索树的后序遍历序列 - 力扣（LeetCode）](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof/)

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。

 

参考以下这颗二叉搜索树：

     5
    / \
   2   6
  / \
 1   3
示例 1：

输入: [1,6,3,2,5]
输出: false
示例 2：

输入: [1,3,2,6,5]
输出: true
 

提示：

数组长度 <= 1000

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof
著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。


```
// 答案还没有做
```


```

```


```

```


```

```