---
layout: post
title: LeetCode_Stack_Queue_Problem
date: 2023-06-15
tags: leetcode algorithm csharp learning c++
---

This blog is the learning note of solving leetcode stack&queue related problems

<!--more-->
<!-- <span style="color: blue;"> </span> -->

# <span style="color: blue;"> 栈和队列 Stack & Queue </span>

## <span style="color: blue;"> 232. Implement queue using stacks 利用栈实现队列 </span>

### <span style="color: blue;"> Problem Description 问题描述 </span>

> 利用栈来实现队列的功能，可以使用的方法均为栈方法：

1. `void push(int x)` 
2. `int pop()`
3. `int peek()`
4. `boolean empty()`

### <span style="color: blue;"> 例子 </span>

```
Input
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
Output
[null, null, null, 1, 1, false]

Explanation
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false

```
> 思考：如何让其变为 O(1) 的时间复杂度?

### <span style="color: blue;"> 解题思路 </span>

我们可以准备两个栈，所谓输入栈，输出栈，基本思想就是利用入栈元素顺序和出栈元素顺序相反的规则，利用两个栈进行两次顺序反转使得结果类似队列，如下图所示：

![232]({{site.baseurl}}/assets/img/232.gif)

> CSharp 实现

```csharp
public class MyQueue
{
    Stack<int> inputStack;
    Stack<int> outputStack;
    public MyQueue() 
    {
        inputStack = new Stack<int>();
        outputStack = new Stack<int>();
    }

    public void Push(int item)
    {
        inputStack.Push(item);
    }
    
    public int Pop()
    {
        if (outputStack.Count == 0)
        {
            while (inputStack.Count > 0)
            {
                outputStack.Push(inputStack.Pop());
            }
        }
        
        return outputStack.Pop();
    }

    public int Peek()
    {
        if (outputStack.Count > 0)
        {
            return outputStack.Peek();
        }
        else if (inputStack.Count > 0 && outputStack.Count == 0)
        {
            while (inputStack.Count > 0)
            {
                outputStack.Push(inputStack.Pop());
            }
        }

        return outputStack.Peek();
    }

    public bool Empty()
    {
        return inputStack.Count == 0 && outputStack.Count == 0;
    }
}
```

### <span style="color: blue;"> Remarks </span>

1. 注意Pop()操作时的顺序，先确保`outputStack`是空的，再去检查`inputStack`
2. `Empty()`只需判断两个栈都为空即可
3. 时间复杂度为 `O(1)`，空间复杂度 `O(n)`

## <span style="color: blue;"> 225. Implement stack using queues 利用队列实现栈 </span>

### <span style="color: blue;"> Problem Description 问题描述 </span>

> 利用队列的方法来实现栈的功能：

1. `void push(int x)` 
2. `int pop()`
3. `int top()`
4. `boolean empty()`

### <span style="color: blue;"> 例子 </span>

```
Input
["MyStack", "push", "push", "top", "pop", "empty"]
[[], [1], [2], [], [], []]
Output
[null, null, null, 2, 2, false]

Explanation
MyStack myStack = new MyStack();
myStack.push(1);
myStack.push(2);
myStack.top(); // return 2
myStack.pop(); // return 2
myStack.empty(); // return False
```

### <span style="color: blue;"> 解题思路 </span>

和上一题232类似，但也有不同，我们也是可以使用两个队列，一个用来输出，一个用来备份除栈底元素以外的其他元素，具体思路如下图所示

![225]({{site.baseurl}}/assets/img/225.gif)

> 时间复杂度`Push()`和`Top()`方法为`O(n)`其他为`O(1)`

> 其他优化：

1. 只使用一个队列，相当于需要通过`queue.count`来找到栈底元素
2. 在有指针的语言中，可以简化备份过程，即将两个队列的值交换一下：
   ```
   queue tmpqueue = backupqueue
   backupqueue = outputqueue
   outputqueue = tmpqueue
   ```

> C#实现

```csharp
public class MyStack {

    public Queue<int> outputQueue;
    public Queue<int> backupQueue;

    public MyStack()
    {
        outputQueue = new Queue<int>();
        backupQueue = new Queue<int>();
    }

    public void Push(int x)
    {
        outputQueue.Enqueue(x);
    }

    public int Pop()
    {
        if (backupQueue.Count == 0)
        {
            while (outputQueue.Count > 1)
            {
                backupQueue.Enqueue(outputQueue.Dequeue());
            }
        }

        while (backupQueue.Count > 0)
        {
            outputQueue.Enqueue(backupQueue.Dequeue());
        }

        return outputQueue.Dequeue();
    }

    public int Top()
    {
        while (outputQueue.Count > 1)
        {
            backupQueue.Enqueue(outputQueue.Dequeue());
        }
        int result = outputQueue.Dequeue();
        while (backupQueue.Count > 0)
        {
            outputQueue.Enqueue(backupQueue.Dequeue());
        }
        outputQueue.Enqueue(result);

        return result;
    }

    public bool Empty()
    {
        return outputQueue.Count == 0 && backupQueue.Count == 0;
    }
}
```

## <span style="color: blue;"> 20. Valid Parentheses 有效括号 </span>

### <span style="color: blue;"> Problem Description 问题描述 </span>

> Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.


### <span style="color: blue;"> 例子 </span>
```
Example 1:
    Input: s = "()" Output: true
Example 2:
    Input: s = "()[]{}" Output: true
Example 3:
    Input: s = "(]" Output: false
```

> C++ 实现 快来看看我写的丑陋的代码😄

```cpp
#include <string>
#include <iostream>
#include <stack>

using namespace std;

class Solution {

    public:
        static bool isValid(string s) {

            stack<char> checker; // declare an empty stack
            for (int i = 0; i < s.size(); i++)
            {
                if (s[i] == '(' || s[i] == '[' || s[i] == '{')
                {
                    checker.push(s[i]);
                }

                if (s[i] == ')' || s[i] == ']' || s[i] == '}')
                {
                    if (!checker.empty())
                    {
                        if (checker.top() == '(' && s[i] == ')')
                        {
                            checker.pop();
                        }else if (checker.top() == '[' && s[i] == ']')
                        {
                            checker.pop();
                        }else if (checker.top() == '{' && s[i] == '}')
                        {
                            checker.pop();
                        }
                        else return false;
                    }
                    else{
                        return false;
                    } 
                }
            }
            
            return checker.empty(); 
        }
};

int main() {

    string s = "(])";
    cout << Solution::isValid(s);
    return 0;
}
```
> 看看人家优雅的代码
```cpp 
class Solution {
public:
    bool isValid(string s) {
        if (s.size() % 2 != 0) return false; // 如果s的长度为奇数，一定不符合要求
        stack<char> st;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(') st.push(')');
            else if (s[i] == '{') st.push('}');
            else if (s[i] == '[') st.push(']');
            // 第三种情况：遍历字符串匹配的过程中，栈已经为空了，没有匹配的字符了，说明右括号没有找到对应的左括号 return false
            // 第二种情况：遍历字符串匹配的过程中，发现栈里没有我们要匹配的字符。所以return false
            else if (st.empty() || st.top() != s[i]) return false;
            else st.pop(); // st.top() 与 s[i]相等，栈弹出元素
        }
        // 第一种情况：此时我们已经遍历完了字符串，但是栈不为空，说明有相应的左括号没有右括号来匹配，所以return false，否则就return true
        return st.empty();
    }
};
```
时间和空间复杂度都是: O(n);