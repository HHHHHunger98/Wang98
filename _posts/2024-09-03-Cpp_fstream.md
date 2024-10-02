---
layout: post
usemathjax: true
title: Cpp_fstream
date: 2024-09-03
tags: learning cpp
---

<!--# <span style="color: blue;"></span>-->
## <span style="color: blue;">C++ : fstream</span>

**fstream**: Input/output stream class to operate on files.

fstream objects maintain a **filebuf** object for input/output operations.

Usage: file streams are associated by constructor or using **open** method(argument **mode** specifies the requested operation mode, flags can be combined with the bitwise OR operator "|"
).

```cpp
#include <fstream>
...
std::fstream fs ("test.txt", std::fstream::in | std::fstream::out);

// or you can use open to associate the fs with a file
fs.open("test.txt", std::fstream::in | std::fstream::out | std::fstream::app);
...

fs.close();
```

<!--more-->
## <span style="color: blue;">Use case 1: get lines from text file</span>

1. To get lines from a input stream, **getline** function can be used.
   
> Notably, ```string::basic_string::geline()``` and ```io::basic_istream::geline()``` are two different API that can be used to handle this problem.
> 
> [1] https://en.cppreference.com/w/cpp/string/basic_string/getline
> 
> [2] https://en.cppreference.com/w/cpp/io/basic_istream/getline

to read a line from text, we can simply use the first API:
```cpp
...
fstream fs("test.txt", fstream::in);
        
if(fs.is_open())
{   
    string lineIterator;
    while (std::istream::getline(fs, lineIterator))
    {
        
    }
    ...
}
```

## <span style="color: blue;">Problem: A + B 1</span>

> Problem description:
- The goal is to calculate the sum of A + B,

- Input: input consists of a series of a and b pairs separated by spaces. One pair of a and b occupies one line. 
- output: For each pair of a and b in the input, you need to output the sum of a and b in turn. For example, for the second pair of a and b in the input, their sum should also be in the second line in the output.
> Example:

```
Input:
3 4
11 40

output:
7
51
```

> Solution：

```cpp
#include <iostream>
#include <string>
#include <fstream>

using namespace std;

class Solution
{
public:
    void AplusB1()
    {
        int sum;
        ifstream fs;
        fs.open("test.txt");
        if(fs.is_open())
        {   
            char num;
            string tmp;
            while (fs.get(num))
            {
                if (num == ' ')
                {
                    sum += stoi(tmp);
                    tmp.clear();
                    continue;
                }
                if (num == '\n')
                {   
                    sum += stoi(tmp);
                    cout << sum << endl;
                    sum = 0;
                    tmp.clear();
                    continue;
                }
                tmp.append(string(1,num));
            }
        }  
        fs.close();
    }
};

int main()
{
    Solution s;
    s.AplusB1();
    return 0;
}
```
