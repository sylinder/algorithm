### 二叉树的前序遍历

- 题目： 给你二叉树的根节点 `root` ，返回它节点值的 **前序** 遍历。

- 思路： 略。

  递归形式：

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        preorder(root, result);
        return result;
    }

    private void preorder(TreeNode root, List<Integer> result) {
        if (root == null) {
            return ;
        }
        result.add(root.val);
        preorder(root.left, result);
        preorder(root.right, result);
    }
}
```

​		非递归形式：

```java
class Solution {
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Stack<TreeNode> stack = new Stack<>();
        stack.push(root);
        while (!stack.isEmpty()) {
            TreeNode cur = stack.pop();
            result.add(cur.val);
            if (cur.right != null) {
                stack.push(cur.right);
            }
            if (cur.left != null) {
                stack.push(cur.left);
            }
        }
        return result;
    }
}
```

