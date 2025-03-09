---
title: C++实现线程池
description: 线程池
author: yu
date: 2025-03-09 20:27:05 +0800
categories: [Computer, C++]
tags: [C++]
---

## 线程池

线程池(Thread Pool)。

生产者-消费者模式。

## 基础版本

类`ThreadPool`封装了 线程数组 和 任务队列。

```cpp
#include <vector>
#include <thread>
#include <queue>
#include <functional>
#include <mutex>

template <typename T>
void print(T&& t)
{
	std::cout << t << std::endl;
}

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

