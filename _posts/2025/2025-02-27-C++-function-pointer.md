---
title: C++的函数指针
description: 指针是地址，函数指针也是地址。
author: yu
date: 2025-02-27 16:15:36 +0800
categories: [Computer, C++]
tags: [C++]
---

## 函数指针

指针是地址（存储地址），函数指针也是地址。只不过`函数指针`是指向`函数首地址`的指针。

函数调用会将被调函数所需要的参数按照与被调函数的形参顺序相反的顺序压入栈中，`函数名`就是函数在栈中的首地址。

## auto定义函数指针

```cpp
#include <iostream>

void Hello()
{
    std::cout << "Hello..." << std::endl;
}

void print(std::string ss)
{
    std::cout << ss << std::endl;
}

int sum(int a, int b)
{
    return a + b;
}

int main()
{
    auto hello = Hello;
    hello();

    auto print_func = print;
    print_func("Hello World!");

    auto sum_func = sum;
    std::cout << sum_func(3, 7) << std::endl;
    return 0;
}
```

## C风格的函数指针

```cpp
#include <iostream>

void Hello()
{
    std::cout << "Hello..." << std::endl;
}

void print(std::string& ss)
{
    std::cout << ss << std::endl;
}

int sum(int a, int b)
{
    return a + b;
}

int main()
{
    void(*hello)() = Hello; // 没有形参
    hello();

    void(*print_func)(std::string&) = print; // 无返回值
    std::string str = "Hello World!";
    print_func(str);

    int(*sum_func)(int, int) = sum;  // 有返回值
    std::cout << sum_func(12, 8) << std::endl;
    return 0;
}
```

## typedef, using 定义函数指针

```cpp
#include <iostream>

typedef void(*typedef_hello)();
void HelloWorld()
{
    std::cout << "Hello World!" <<std::endl;
}

using using_print = void(*)(int);
void printInt(int v)
{
    std::cout << "int: " << v <<std::endl;
}

int main()
{
    typedef_hello hello = HelloWorld;
    hello();
    using_print print = printInt;
    print(7);
    return 0;
}
```

## 函数做为参数

在C++中，函数可以做为参数传递给另一个函数。

在C++ 中，`回调`是一种常见的设计模式,它允许将函数作为参数传递给另一个函数,从而在某个事件发生时调用该函数。

作为参数的函数称为`回调函数`，或者说`回调函数`是一段作为参数传递给其他函数的可执行代码（函数）。

```cpp
#include <iostream>
#include <vector>

void print(int v)
{
    std::cout << "Value: " << v << std::endl;
}

void ForEach(const std::vector<int>& values, void(*print_func)(int))
{
    for(int v : values)
        print_func(v);
}

int main()
{
    std::vector<int> values = {1, 2, 5, 4, 3};
    ForEach(values, print);
    return 0;
}
```
