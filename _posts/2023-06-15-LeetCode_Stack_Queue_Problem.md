---
layout: post
usekatex: false
title: LeetCode_Stack_Queue_Problem
date: 2023-06-15
tags: leetcode algorithm csharp learning cpp
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

## <span style="color: blue;"> 150. Evaluate Reverse Polish Notation 逆波兰表达式求值 </span>

### <span style="color: blue;"> Problem Description 问题描述 </span>

输入为tokens的数组，里面是一个表达式的逆波兰形式，输出为该表达式的结果

### <span style="color: blue;"> 例子 </span>

```
Example 1:

Input: tokens = ["2","1","+","3","*"]
Output: 9
Explanation: ((2 + 1) * 3) = 9

Example 2:

Input: tokens = ["4","13","5","/","+"]
Output: 6
Explanation: (4 + (13 / 5)) = 6

Example 3:

Input: tokens = ["10","6","9","3","+","-11","*","/","*","17","+","5","+"]
Output: 22
Explanation: ((10 * (6 / ((9 + 3) * -11))) + 17) + 5
= ((10 * (6 / (12 * -11))) + 17) + 5
= ((10 * (6 / -132)) + 17) + 5
= ((10 * 0) + 17) + 5
= (0 + 17) + 5
= 17 + 5
= 22

```

### <span style="color: blue;"> 解题思路 </span>

如果利用栈来实现就会变得很简单，遍历tokens数组，碰到数字就压栈，碰到运算符就出栈栈顶的两个数字，并将运算结果压栈

> C++ 实现 快来看看我写的丑陋的代码😄

remarks: 简单说明一下，以下代码就是简单暴力的解决问题，可以优化，比如优化一下分支情况，优化一下类型转化，太多类型转换有点费时间和资源

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <stack>

using namespace std;

class Solution {

    public:
        static int evalRPN (vector<string>& tokens) {

            stack<string> calculator;
            for (int i = 0; i < tokens.size(); i++)
            {
                if (tokens[i] == "+" || tokens[i] == "-" || tokens[i] == "*" || tokens[i] == "/")
                {
                    int right = stoi(calculator.top());
                    calculator.pop();
                    int left = stoi(calculator.top());
                    calculator.pop();
                    if (tokens[i] == "+")
                    {
                        calculator.push(to_string(left + right));
                    }else if (tokens[i] == "-")
                    {
                        calculator.push(to_string(left - right));
                    }else if (tokens[i] == "*")
                    {
                        calculator.push(to_string(left * right));
                    }else if (tokens[i] == "/")
                    {
                        calculator.push(to_string(left / right));
                    }
                }
                else {
                    calculator.push(tokens[i]);
                }
            }
            return stoi(calculator.top());
        }
};
int main () {

    vector<string> tokens = {"10","6","9","3","+","-11","*","/","*","17","+","5","+"};
    cout << Solution::evalRPN(tokens);
    return 0;
}
```

- 时间复杂度: O(n)
- 空间复杂度: O(n)

## <span style="color: blue;"> 239. Sliding Window Maximum 滑动窗口最大值 </span>

### <span style="color: blue;"> Problem Description 问题描述 </span>

给一个数组，以及一个大小为k的滑动窗口，窗口从左到右步长为1进行滑动，输出为窗口中最大值序列

### <span style="color: blue;"> 例子 </span>

```
Example 1:

Input: nums = [1,3,-1,-3,5,3,6,7], k = 3
Output: [3,3,5,5,6,7]
Explanation: 
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7

Example 2:

Input: nums = [1], k = 1
Output: [1]
```

### <span style="color: blue;"> 解题思路 </span>

1. 暴力解法，时间复杂度为O(n*k)，每次滑窗移动时遍历滑窗，找到最大值
   
2. 特别解法，简单来说就是要维护一个单调队列，为什么这么说，因为我们只需要找到滑动窗口中最大的值就行，没有必要维护所有的值，因此我们可以设计一个这样的单调队列，简单的接口就是：
```cpp
void pop(int value){
    //当队首的元素等于将要去除的值时，就pop,否则不进行操作
}
void push(int value){
    //比较队尾的元素和入队元素，如果队尾元素小于入队元素，就将队尾元素pop_back()，直到队尾元素大于入队元素或者队列为空时，再加入入队元素
}
```
因为nums[]数组中的每个元素最多也就被 push_back 和 pop_back 各一次，时间复杂度: O(n)，空间复杂度: O(k)

参考：<herf>https://github.com/youngyangyang04/leetcode-master/blob/master/problems/0239.%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3%E6%9C%80%E5%A4%A7%E5%80%BC.md</herf>

```cpp
#include <iostream>
#include <vector>
#include <string>
#include <queue>

using namespace std;
class MyQue {
    public:
        deque<int> myque;
        
        void pop (int value) {
            if (!myque.empty() && value == myque.front())
            {
                myque.pop_front();
            }
        }

        void push (int value) {
            while (!myque.empty() && myque.back() < value)
            {
                myque.pop_back();
            }
            myque.push_back(value);
        }

        int front () {
            return myque.front();
        }
};
class Solution {
    public:
        static vector<int> maxSlidingWindow(vector<int>& nums, int k) {
            MyQue slidMax;
            vector<int> result;

            for (int i = 0; i < k; i++)
            {
                slidMax.push(nums[i]);
            }

            result.push_back(slidMax.front());

            for (int i = k; i < nums.size(); i++)
            {
                slidMax.pop(nums[i-k]);
                slidMax.push(nums[i]);
                result.push_back(slidMax.front());
            }

            return result;
        }
};

int main () {

    vector<int> nums = {1,3,1,2,0,5};
    int k = 3;

    vector<int> result = Solution::maxSlidingWindow(nums, k);
    for (int i = 0; i < result.size(); i++)
    {
        cout << result[i];
    }
    
    return 0;
}
```

## <span style="color: blue;"> 347. Top K Frequent Elements Top k 个高频元素 </span>

### <span style="color: blue;"> Problem Description 问题描述 </span>

给一个数组，返回出现最多的前k个元素的数组

### <span style="color: blue;"> 例子 </span>

```
Example 1:

Input: nums = [1,1,1,2,2,3], k = 2
Output: [1,2]

Example 2:

Input: nums = [1], k = 1
Output: [1]
```

### <span style="color: blue;"> Solution 解题思路 </span>

