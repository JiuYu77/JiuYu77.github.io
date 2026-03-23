---
title: C++ delete this
description: delete this，栈对象、堆对象。
author: yu
date: 2025-05-03 16:18:41 +0800
categories: [C++, 基础进阶]
tags: [C++]
---



## 前言

`delete this;` 是特殊场景下 `类`/`结构体` 成员函数中可以使用的操作，对于栈对象、堆对象有所区别。

## 栈对象

`栈对象` 存储在栈内存空间，在成员函数中调用 `delete this;` 会出错（**free(): invalid pointer**），并**终止程序**。

这是因为
- delete 用于动态内存管理，释放 堆内存。
- 栈对象存储在栈内存，由编译器自动管理，不需要手动释放。
- 这种内存管理方式的不匹配会导致“`free(): invalid pointer`”错误。

## 堆对象

`堆对象` 存储在堆内存空间，通常使用 new/delete 或 智能指针（Smart Pointer）来管理。

`delete this;` 用于释放 堆(heap)内存，即对象存储在堆空间，通过指针访问成员函数 和 成员变量。

`delete this;` 的本质是**调用一个或多个析构函数，然后释放内存**。

1. **类/结构体 的析构函数中使用delete this，会发生什么？**
- 在析构函数中使用 delete this时，析构函数调用 `delete this`，delete this 又调用 `析构函数`，形成无限递归，造成堆栈溢出，系统崩溃（Linux 下出现段错误`Segmentation fault(core dumped)`），因此**不能在析构函数中使用 delete this**。

2. **`静态成员函数`中不能使用 delete this**。静态成员函数 为所有类对象共享，不属于某一个类实例，没有this指针。

3. **其他成员函数使用 delete this，会发生什么？**
- 非析构函数、非静态成员函数， 中**可以使用** `delete this`，这会调用析构函数释放资源，接着释放对象内存，指向对象的指针变为 悬挂指针(Dangling Pointer)。
- 之后仍然可以通过 对象指针 调用 成员函数。
- 只不过`non-static 成员变量`中的数据已经不正确了，若成员函数中使用了`非静态成员变量`，则得不到预期的结果。
- `static成员变量` 可以正常使用，因为它为所有类对象共享，对象存储在堆区，但静态成员变量存储在 静态存储区 或 只读数据段。

