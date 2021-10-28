### 最小栈

- 题目： 设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈。
  - push(x) —— 将元素 x 推入栈中。
  - pop() —— 删除栈顶的元素。
  - top() —— 获取栈顶元素。
  - getMin() —— 检索栈中的最小元素。
- 思路： 使用两个栈，一个用来正常保存数据，一个用来保存当前最小值。

```java
class MinStack {

    private Stack<Integer> valueStack;
    private Stack<Integer> minStack;

    public MinStack() {
        valueStack = new Stack<>();
        minStack = new Stack<>();
    }
    
    public void push(int val) {
        if (minStack.isEmpty()) {
            minStack.push(val);
        } else {
            minStack.push(val > minStack.peek() ? minStack.peek() : val);
        }
        valueStack.push(val);
    }
    
    public void pop() {
        if (valueStack.isEmpty()) {
            throw new RuntimeException("Stack is empty");
        }
        minStack.pop();
        valueStack.pop();
    }
    
    public int top() {
        if (valueStack.isEmpty()) {
            throw new RuntimeException("Stack is empty");
        }
        return valueStack.peek();
    }
    
    public int getMin() {
        if (minStack.isEmpty()) {
            throw new RuntimeException("stack is empty");
        }
        return minStack.peek();
    }
}
```



### 有效的括号

- 题目： 给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

  有效字符串需满足：

  左括号必须用相同类型的右括号闭合。
  左括号必须以正确的顺序闭合。

- 思路： 遍历一遍S，遇到左括号，将对应的右括号push到栈中。遇到右括号，将栈顶元素和当前元素相比，栈为空或者不等就不是有效的括号。遍历完S之后，如果栈为空，则是有效的括号，否则无效。

```java
class Solution {
    public boolean isValid(String s) {
        if (s == null || s.length() == 0) {
            return true;
        }
        Stack<Character> stack = new Stack<>();
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if (ch == '(') {
                stack.push(')');
            } else if (ch == '[') {
                stack.push(']');
            } else if (ch == '{') {
                stack.push('}');
            } else if (stack.isEmpty() || ch != stack.pop()) {
                return false;
            } 
        }
        return stack.isEmpty();
    }
}
```

