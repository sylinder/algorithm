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

