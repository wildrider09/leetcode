---
comments: true
difficulty: Easy
edit_url: https://github.com/doocs/leetcode/edit/main/lcci/03.02.Min%20Stack/README_EN.md
---

<!-- problem:start -->

# [03.02. Min Stack](https://leetcode.cn/problems/min-stack-lcci)

[Chinese Version](/lcci/03.02.Min%20Stack/README.md)

## Description

<!-- description:start -->

<p>How would you design a stack which, in addition to push and pop, has a function min which returns the minimum element? Push, pop and min should all operate in 0(1) time.</p>

<p><strong>Example: </strong></p>

<pre>

MinStack minStack = new MinStack();

minStack.push(-2);

minStack.push(0);

minStack.push(-3);

minStack.getMin();   --&gt; return -3.

minStack.pop();

minStack.top();      --&gt; return 0.

minStack.getMin();   --&gt; return -2.</pre>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Double Stack

We use two stacks to implement this, where `stk1` is used to store data, and `stk2` is used to store the current minimum value in the stack. Initially, `stk2` stores a very large value.

- When we push an element `x` into the stack, we push `x` into `stk1`, and push `min(x, stk2[-1])` into `stk2`.
- When we pop an element from the stack, we pop the top elements of both `stk1` and `stk2`.
- When we want to get the top element in the current stack, we just need to return the top element of `stk1`.
- When we want to get the minimum value in the current stack, we just need to return the top element of `stk2`.

For each operation, the time complexity is $O(1)$, and the space complexity is $O(n)$.

<!-- tabs:start -->

#### Java

```java
class MinStack {
    private Deque<Integer> stk1 = new ArrayDeque<>();
    private Deque<Integer> stk2 = new ArrayDeque<>();

    /** initialize your data structure here. */
    public MinStack() {
        stk2.push(Integer.MAX_VALUE);
    }

    public void push(int x) {
        stk1.push(x);
        stk2.push(Math.min(x, stk2.peek()));
    }

    public void pop() {
        stk1.pop();
        stk2.pop();
    }

    public int top() {
        return stk1.peek();
    }

    public int getMin() {
        return stk2.peek();
    }
}

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack obj = new MinStack();
 * obj.push(x);
 * obj.pop();
 * int param_3 = obj.top();
 * int param_4 = obj.getMin();
 */
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
