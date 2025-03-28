---
title: C++实现线程池
description: 多线程、线程池
author: yu
date: 2025-03-09 20:27:05 +0800
categories: [C++, 线程]
tags: [C++]
---

## 线程池

**`线程池`(Thread Pool)** 用于管理多个线程，这些线程 并发 或 并行执行。

线程池实际上是在内部构建了一个**生产者-消费者模型**(一个非常经典的多线程并发协作的模型)。
生产者`产生任务`并插入任务队列，消费者`消耗任务`(从任务队列取走任务)并完成。

## 基础版本

类`ThreadPool`封装了 线程数组、任务队列、任务入队列函数。

```cpp
#include <vector>
#include <thread>
#include <queue>
#include <functional>
#include <mutex>


class ThreadPool
{
private:
	unsigned int m_threads; // number of threads
	std::vector<std::thread> m_threadPool; // thread array
	
	std::queue<std::function<void()>> m_tasks; // task queue
	std::mutex m_mutex;
	std::condition_variable m_condition;

	bool m_stop; // stop flag for thread

public:
    ThreadPool(unsigned int thread_num)
    : m_threads(thread_num), m_stop(false)
    {
        unsigned int threads = std::thread::hardware_concurrency(); // get the number of CPU cores
        if (m_threads > threads && threads != 0) {
            m_threads = threads;  // 防止线程数超过CPU核心数
        }  // 根据具体情况，当前行及以上三行（共四行代码）可以保留，也可 删除

        for (int i = 0; i < m_threads; i++)
        {
            m_threadPool.emplace_back([this]()
            {
                while (true)
                {
                    std::function<void()> task;
                    {
                        std::unique_lock<std::mutex> lock(m_mutex);
                        m_condition.wait(lock, [this]()
                            {
                                return m_stop || !m_tasks.empty();
                            });
                        if (m_stop && m_tasks.empty())
                            return;
                        task = std::move(m_tasks.front());
                        m_tasks.pop();
                    }
                    task();
                }
            });
        }
    }

    ~ThreadPool()
    {
        {
            std::unique_lock<std::mutex> lock(m_mutex);
            m_stop = true;
        }
        m_condition.notify_all();
        for (auto& thread : m_threadPool)
        {
            thread.join();
        }
    }

    // add task to the queue
    template <typename F, typename... Args>
    void enqueue(F&& f, Args&&... args)
    {
        auto task = std::bind(std::forward<F>(f), std::forward<Args>(args)...);
        {
            std::unique_lock<std::mutex> lock(m_mutex);
            m_tasks.emplace(task);
        }
    }
};
```

测试上面的线程池`ThreadPool`：
```cpp
#include <iostream>
#include <string>

int main() 
{
    ThreadPool t_pool(4);
    for (int i = 0; i < 8; i++) {
        t_pool.enqueue([](int i, int j) {
            std::string ss = std::string("[task: ") + std::to_string(i) + " start...] ";
            std::cout << ss << '\n';
            std::this_thread::sleep_for(std::chrono::seconds(2));
            std::string sss = std::string("[task: ") + std::to_string(i) + " end] ";
            std::cout << sss << "\n";
        }, i, i + 1);
    }
    return 0;
}
```

## 单例版本

将`ThreadPool`改造为**单例模式**。

```cpp

```

## 多线程程序一定就好吗？

不一定，一个程序设计为单线程还是多线程，要看具体的应用场景：

**IO密集型**  
IO密集型程序，会进行大量IO操作，比如设备、文件、网络操作。

无论是单核CPU、多核CPU、多个CPU，都比较`适合`采用多线程。

**CPU密集型**  
CPU密集型程序，其指令大多是做计算用的。
- 单核CPU  
多线程存在上下文切换，会带来额外开销，线程越多上下文切换额外花费的时间也越多，还不如一个线程一直进行计算的效率高，`不适合`多线程。
- 多核CPU、多个CPU
多个线程可以并行执行，对CPU利用率好，可以采用多线程。

## 线程不是越多越好

为了完成任务，创建很多的线程可以吗？线程真的越多越好吗？  
`线程不是越多越好`，可从一下方面分析：
- 线程的创建和销毁都是非常“重”的操作：空间的切换、内存分配（线程PCB、线程的内核栈等）
- 线程栈本身占用大量内存：创建了一大批线程，每一个线程都需要线程栈，栈几乎都被占用完了，无法处理实际业务
- 线程的上下文切换要占用大量时间：线程过多，线程上下文切换花费的CPU时间也特别多，导致CPU的利用率降低
- 大量线程同时唤醒会使系统经常出现锯齿状负载或瞬间负载量很大导致宕机

C++的muduo、libevent，Java的Netty、mina等开源网络库，通过IO复用 + 多线程实现，它们的`线程数量`一般都是按照当前`CPU的核心数量`来确定的。

