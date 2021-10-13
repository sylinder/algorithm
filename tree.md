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



### 二叉树的中序遍历

- 题目： 给定一个二叉树的根节点 `root` ，返回它的 **中序** 遍历。
- 思路： 略。

​		递归形式：

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        inorder(root, result);
        return result;
    }

    private void inorder(TreeNode root, List<Integer> list) {
        if (root == null) {
            return ;
        }
        inorder(root.left, list);
        list.add(root.val);
        inorder(root.right, list);
    }
}
```

​		非递归形式：

```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Stack<TreeNode> stack = new Stack<>();
        while (root != null || !stack.isEmpty()) {
            while (root != null) {
                stack.push(root);
                root = root.left;
            }
            root = stack.pop();
            result.add(root.val);
            root = root.right;
        }
        return result;
    }
}
```



### 二叉树的后序遍历

- 题目： 给定一个二叉树，返回它的 *后序* 遍历。
- 思路： 略。

​		递归形式：

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        postorder(root, result);
        return result;
    }

    private void postorder(TreeNode root, List<Integer> list) {
        if (root == null) {
            return ;
        }
        postorder(root.left, list);
        postorder(root.right, list);
        list.add(root.val);
    }
}
```

​		非递归形式：

```java
class Solution {
    public List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Stack<TreeNode> stack1 = new Stack<>();
        Stack<TreeNode> stack2 = new Stack<>();
        stack1.push(root);
        while (!stack1.isEmpty()) {
            root = stack1.pop();
            stack2.push(root);
            if (root.left != null) {
                stack1.push(root.left);
            }
            if (root.right != null) {
                stack1.push(root.right);
            }
        }
        while (!stack2.isEmpty()) {
            result.add(stack2.pop().val);
        }
        return result;
    }
}
```



### 二叉树的层序遍历

- 题目： 给你一个二叉树，请你返回其按 **层序遍历** 得到的节点值。 （即逐层地，从左到右访问所有节点）。
- 思路： 略。

```java
class Solution {
    public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> list = new ArrayList<>();
            while (size-- > 0) {
                TreeNode cur = queue.poll();
                list.add(cur.val);
                if (cur.left != null) {
                    queue.add(cur.left);
                }
                if (cur.right != null) {
                    queue.add(cur.right);
                }
            }
            result.add(list);
        }
        return result;
    }
}
```



### N叉树的层次遍历

- 题目： 给定一个 N 叉树，返回其节点值的*层序遍历*。（即从左到右，逐层遍历）。
- 思路： 略。

```java
class Solution {
    public List<List<Integer>> levelOrder(Node root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Queue<Node> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> list = new ArrayList<>();
            while (size-- > 0) {
                root = queue.poll();
                list.add(root.val);
                queue.addAll(root.children);
            }
            result.add(list);
        }
        return result;
    }
}
```





### 二叉树的右视图

- 题目： 给定一个二叉树的 **根节点** `root`，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。
- 思路： 层次遍历，然后取每一层最后一个即可。

```java
class Solution {
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Queue<TreeNode> queue = new LinkedList<>();
        queue.add(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            while (size > 0) {
                root = queue.poll();
                if (size == 1) {
                    result.add(root.val);
                }
                if (root.left != null) {
                    queue.add(root.left);
                }
                if (root.right != null) {
                    queue.add(root.right);
                }
                size--;
            }
        }
        return result;
    }
}
```





### 二叉树的锯齿形层次遍历

- 题目： 给定一个二叉树，返回其节点值的锯齿形层序遍历。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。
- 思路： 使用两个栈， stack1用在奇数层，stack2用在偶数层。奇数层的时候将节点从stack1中pop出来，并将对应的子节点从左到右push到stack2中；偶数层的时候将节点从stack2中pop出来，并将对应的子节点从右向左push到stack1中，直到stack1和stack2都为空。

```java
class Solution {
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> result = new ArrayList<>();
        if (root == null) {
            return result;
        }
        Stack<TreeNode> oddStack = new Stack<>();
        Stack<TreeNode> evenStack = new Stack<>();
        oddStack.push(root);
        int level = 1;
        while (!oddStack.isEmpty() || !evenStack.isEmpty()) {
            List<Integer> list = new ArrayList<>();
            if (level % 2 == 1) {
                while (!oddStack.isEmpty()) {
                    root = oddStack.pop();
                    list.add(root.val);
                    if (root.left != null) {
                        evenStack.push(root.left);
                    }
                    if (root.right != null) {
                        evenStack.push(root.right);
                    }
                }
            } else {
                while (!evenStack.isEmpty()) {
                    root = evenStack.pop();
                    list.add(root.val);
                    if (root.right != null) {
                        oddStack.push(root.right);
                    }
                    if (root.left != null) {
                        oddStack.push(root.left);
                    }
                }
            }
            result.add(list);
            level++;
        }
        return result;
    }
}
```





### 在二叉树中找到两个节点的最近公共祖先

- 题目： 给定一棵二叉树(保证非空)以及这棵树上的两个节点对应的val值 o1 和 o2，请找到 o1 和 o2 的最近公共祖先节点。
- 思路： 略。

```java
public class Solution {
    public int lowestCommonAncestor (TreeNode root, int o1, int o2) {
        TreeNode node = findNode(root, o1, o2);
        if (node == null) {
            return -1;
        }
        return node.val;
    }
    
    private TreeNode findNode(TreeNode root, int o1, int o2) {
        if (root == null) {
            return null;
        }
        if (root.val == o1 || root.val == o2) {
            return root;
        }
        TreeNode left = findNode(root.left, o1, o2);
        TreeNode right = findNode(root.right, o1, o2);
        if (left != null && right != null) {
            return root;
        }
        return left == null ? right : left;
    }
}
```

