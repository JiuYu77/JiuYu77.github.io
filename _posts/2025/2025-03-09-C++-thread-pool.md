---
title: C++实现线程池
description: 线程池
author: yu
date: 2025-03-09 20:27:05 +0800
categories: [Computer, C++]
tags: [C++]
---

## 线程池

**`线程池`(Thread Pool)** 用于管理多个线程，这些线程 并发 或 并行执行。

线程池实际上是在内部构建了一个**生产者-消费者模型**。
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
        std::cout << "CPU Cores: " << threads << std::endl;
        if (m_threads > threads && threads != 0) {
            m_threads = threads;
        }
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
