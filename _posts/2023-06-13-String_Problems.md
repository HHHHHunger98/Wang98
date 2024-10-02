---
layout: post
usemathjax: false
title: String_Problems
date: 2023-06-13
tags: algorithm csharp
---

<!-- <span style="color: blue;"> </span> -->
This article is a subpart of the learning diary

In this article, we focus on solving leetcode string problems

<!--more-->


# String Problems

## 344. Reverse String.

> Description: Take an array of char elements, and return the reversed char array as output.
>
```
Input: s = ["h","e","l","l","o"]
Output: ["o","l","l","e","h"]
restriction: solve it in-place, O(1) space complexity
```
Comments on this problem: Kind of easy, even through we have `Array.Reverse(charArray)` to use. We can easily use the two-indexers methods to exchange the elements position.

![344]({{site.baseurl}}/assets/img/344.gif)

```c sharp
public static void ReverseString(char[] s)
{
    int head = 0, tail = s.Length - 1;
    while (head < tail)
    {
        char c = s[head];
        s[head] = s[tail];
        s[tail] = c;
        head++;
        tail--;
    }
}
```

## 541. Reverse String II.

## 151. Reverse Words in String

> Description: Given an input string s, reverse the order of the words. **The output should not contain any redundant spaces.**

```
Input: s = "    the sky    is blue    "
Output: "blue is sky the"
Constraints: Space complexity O(1) if the string data type is mutable in your language.
```

Comments on this problem: String data type is differently implemented in different language. For example, in C or C++, string is a mutable data type, but in C#, string is an immutable class, which is naturally not modifiable. 

- **For immutable string data type**, the possible minimum space complexity is O(n).
- **For mutable string**, we can solve the problem in-place with O(1) space complexity.

We can discuss both solutions for immutable and mutable string data type.

### **Solution for mutable string**
For mutable string, the problem can be solved in 3 steps:
1. Remove redundant spaces.
2. Reverse the whole string.
3. Reverse every word.

```c#
For example:
Input: "  the sky is   blue  "
step1: "the sky is blue"
step2: "eulb si yks eht"
step3: "blue is sky the" 
```
1. step1: use two-indexes method(fast pointer and slow pointer, 双指针法) to remove all the extra spaces, let fast pointer to traverse the string, let the slow pointer to record all chars that are not equal to ' '. 
2. step2: use two-indexes(left and right, 同样是双指针法). Switch the elements from two end of the string. See **Problem 344. Reverse String**
3. step3: use two-indexes methods to reverse each word.

```c++
class Solution {
public:
    void reverse(string& s, int start, int end){ //翻转，区间写法：左闭右闭 []
        for (int i = start, j = end; i < j; i++, j--) {
            swap(s[i], s[j]);
        }
    }

    void removeExtraSpaces(string& s) {//去除所有空格并在相邻单词之间添加空格, 快慢指针。
        int slow = 0;   //整体思想参考https://programmercarl.com/0027.移除元素.html
        for (int i = 0; i < s.size(); ++i) { //
            if (s[i] != ' ') { //遇到非空格就处理，即删除所有空格。
                if (slow != 0) s[slow++] = ' '; //手动控制空格，给单词之间添加空格。slow != 0说明不是第一个单词，需要在单词前添加空格。
                while (i < s.size() && s[i] != ' ') { //补上该单词，遇到空格说明单词结束。
                    s[slow++] = s[i++];
                }
            }
        }
        s.resize(slow); //slow的大小即为去除多余空格后的大小。
    }

    string reverseWords(string s) {
        removeExtraSpaces(s); //去除多余空格，保证单词之间之只有一个空格，且字符串首尾没空格。
        reverse(s, 0, s.size() - 1);
        int start = 0; //removeExtraSpaces后保证第一个单词的开始下标一定是0。
        for (int i = 0; i <= s.size(); ++i) {
            if (i == s.size() || s[i] == ' ') { //到达空格或者串尾，说明一个单词结束。进行翻转。
                reverse(s, start, i - 1); //翻转，注意是左闭右闭 []的翻转。
                start = i + 1; //更新下一个单词的开始下标start
            }
        }
        return s;
    }
};
```

### **Solution for immutable string**

In C#, the `String` data type is immutable, therefore it's impossible to solve the problem in-place, but we can use `StringBuilder` class in order to reduce space consumption as much as possible.

The problem can be solved in one traversal:
1. Traverse the string from the end. 
2. Using two indexes to record the words encountered during the traversal.
3. Use a StringBuilder instance to store the result.

```C#
public static string ReverseWords(string s)
        {
            StringBuilder result = new StringBuilder();
            int start = s.Length - 1, end = s.Length - 1;

            while (start >= 0)
            {
                while (end >= 0 && s[end] == ' ')
                {
                    end--;
                }
                start = end;
                while (start >= 0 && s[start] != ' ')
                {
                    start--;
                }

                for (int i = start+1; i <= end; i++)
                {
                    result.Append(s[i]);
                }
                if (end - start > 0)
                {
                    result.Append(' ');
                }
                end = start;
            }
            if (result[result.Length-1] == ' ') result.Length--;
            
            return result.ToString();
        }
```

## 剑指offer 58-II. 左旋转字符串

> Description: Write a method to left rotate a string. **Find a way to solve the problem in-place! ( Time complexity O(1) )**

```c#
示例 1：
输入: s = "abcdefg", k = 2
输出: "cdefgab"

示例 2：
输入: s = "lrloseumgh", k = 6
输出: "umghlrlose"

限制：
1 <= k < s.length <= 10000
```

Comments on this problem: We can solve it in a magnificent way by reversing the string twice. **Overall Reversal + Partial Reversal ( 整体反转 + 局部反转 )** As we already know how to reverse the string, we can just reverse the first k elements substring, and reverse the left substring. At the end we reverse the whole string again.

```C#
For example:
Input: string str = "abcdefg", int k = 2
Reverse the substrings: "bagfedc"
Reverse the whole string: "cdefgab"
```
### Solution
```C#
public static string LeftRotateString(string s, int k)
{
    char[] chars = s.ToCharArray();
    for (int i = 0, j = k - 1; i < j; i++, j--)
    {
        char tmp = chars[i];
        chars[i] = chars[j];
        chars[j] = tmp;
    }
    for (int i = k, j = chars.Length - 1; i < j; i++, j--)
    {
        char tmp = chars[i];
        chars[i] = chars[j];
        chars[j] = tmp;
    }
    for (int i = 0, j = chars.Length - 1; i < j; i++, j--)
    {
        char tmp = chars[i];
        chars[i] = chars[j];
        chars[j] = tmp;
    }

    return new string(chars);
}
```

## 28. StrStr()

> Description: Given two strings: `string haystack` and `string needle`. Return the index of the first occurrence of `needle`. If `needle` doesn't occur, return `-1`.

```C#
Input: haystack = "sadbutsad", needle = "sad"
Output: 0
Explanation: "sad" occurs at index 0 and 6.
The first occurrence is at index 0, so we return 0.
```
