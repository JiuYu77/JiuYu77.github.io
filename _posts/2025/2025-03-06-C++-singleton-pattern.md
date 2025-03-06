---
title: 单例模式
description: 单例模式下一个类只允许有一个实例（普通变量 或 new的对象）。
author: yu
date: 2025-03-06 16:22:24 +0800
categories: [Computer, 设计模式]
tags: [设计模式, C++]
---

## 单例模式
单例模式（Singleton Pattern）是一种创建型设计模式，它确保一个类只有一个实例，并提供了一个全局访问点来访问该实例。

本文基于C++介绍了几种线程安全的单例模式。

## 基于静态局部变量

基于`静态局部变量`实现的单例模式会**自动调用析构函数（自动释放内存）**。

且这种方法在C++11及之后是线程安全的，C++11之前需要`加锁`实现线程安全以避免资源竞争（重复执行静态局部变量的定义语句）。

### 本身就是线程安全的版本

C++11及之后。
```cpp
class Singleton
{
private:
    Singleton() = default;
    // Singleton() {}
public:
    ~Singleton() {}
    static Singleton* instance();
};

Singleton* Singleton::instance()
{
    static Singleton single; // C++11及之后是线程安全的
    return &single;
}
```

### 本身不是线程安全的版本

#### 使用锁实现线程安全

C++11之前需要`加锁`实现线程安全。

以C++11引入的std::mutex互斥锁为例，C++11之前无法使用，C++11之前可以使用其他的锁（如Linux内核提供的锁）。

```cpp
#include <iostream>
#include <mutex>

class Singleton
{
private:
    static std::mutex m_mutex;  // 互斥锁
    // Singleton() = default;
    Singleton() { std::cout << "Singleton()..." << std::endl; }
public:
    ~Singleton() { std::cout << "~Singleton()..." << std::endl; }
    static Singleton* instance();
};
std::mutex Singleton::m_mutex;

// 直接使用std::mutex
Singleton* Singleton::instance()
{
    m_mutex.lock(); // 以C++11引入的std::mutex互斥锁为例
    std::cout << "lock()..." << std::endl;
    static Singleton single;
    m_mutex.unlock();
    return &single;
}

// 使用std::lock_guard
Singleton* Singleton::instance()
{
    std::lock_guard<std::mutex> guard(m_mutex);
    std::cout << "lock_guard..." << std::endl;
    static Singleton single;
    return &single;
}
```

#### 不使用锁

**不使用锁实现线程安全**，可以将类的唯一实例定义为`静态`（static）成员变量。

```cpp
#include <iostream>

class Singleton
{
private:
    static Singleton m_single;
    // Singleton() = default;
    Singleton() { std::cout << "Singleton()..." << std::endl; }
public:
    ~Singleton() { std::cout << "~Singleton()..." << std::endl; }
    static Singleton* instance();
};
Singleton Singleton::m_single;
Singleton* Singleton::instance()
{
    return &m_single;
}
```

## 基于指针

基于`指针`实现的单例模式，不会自动调用析构函数，可以实现一个**内嵌垃圾回收类**。

### 饿汉实现

饿汉实现：单例类定义的时候就进行了实例化，是`线程安全的`。

#### 饿汉
```cpp
#include <iostream>

class Singleton
{
private:
    static Singleton *m_single;
    Singleton() { std::cout << "Singleton()..." << std::endl; }
public:
    ~Singleton()  // 不会被调用
    { std::cout << "~Singleton()..." << std::endl; }
    static Singleton* instance();
};
Singleton* Singleton::m_single = new Singleton();
Singleton* Singleton::instance()
{
    return m_single;
}
```

#### 实现自动垃圾回收

内嵌圾回收类，自动释放内存。

下面的CGarbo类，就是内嵌圾回收类，Garbo译为垃圾工人。
```cpp
#include <iostream>

class Singleton
{
private:
    static Singleton *m_single;
    class CGarbo;
    static CGarbo m_Cgarbo;
    Singleton() { std::cout << "Singleton()..." << std::endl; }
public:
    ~Singleton() { std::cout << "~Singleton()..." << std::endl; }
    static Singleton* instance();
};

class Singleton::CGarbo
{
public:
    CGarbo() {}
    ~CGarbo()
    {
        if(m_single != nullptr)
            delete m_single;
    }
};
Singleton::CGarbo Singleton::m_Cgarbo;

Singleton* Singleton::m_single = new Singleton();
Singleton* Singleton::instance()
{
    return m_single;
}
```

### 懒汉实现

懒汉实现：第一次用到类实例的时候才会去实例化，不是线程安全的，需要通过`锁机制`实现线程安全。
```cpp
#include <iostream>
#include <mutex>

class Singleton
{
private:
    static Singleton *m_single;
    static std::mutex m_mutex;  // 锁

    class CGarbo  // CGarbo类也可以在 Singleton内部实现
    {
    public:
        CGarbo() {}
        ~CGarbo()
        {
            if(m_single != nullptr)
                delete m_single;  // 释放内存
        }
    };
    static CGarbo m_Cgarbo;
    Singleton() { std::cout << "Singleton()..." << std::endl; }
public:
    ~Singleton() { std::cout << "~Singleton()..." << std::endl; }
    static Singleton* instance();
};

Singleton::CGarbo Singleton::m_Cgarbo;

std::mutex Singleton::m_mutex;
Singleton* Singleton::m_single = nullptr;
Singleton* Singleton::instance()
{
    if(m_single == nullptr)  // 避免多次加锁解锁操作
    {
        std::lock_guard<std::mutex> lock(m_mutex);
        std::cout << "lock_guard..." << std::endl;
        if(m_single == nullptr)
            m_single = new Singleton();
    }
    return m_single;
}
```
