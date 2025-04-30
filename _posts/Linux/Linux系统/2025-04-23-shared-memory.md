---
title: Linux进程间通信之共享内存
description: 共享内存是实现同机进程间通信（IPC）的一种高效方式，它允许多个进程直接访问同一块物理内存区域，避免了数据复制的开销。
author: yu
date: 2025-04-23 15:37:00 +0800
categories: [Linux, Linux系统编程]
tags: [Linux]
---

## 前言

`共享内存`（Shared Memory）是实现**同机**`进程间通信`（Inter-Process Communication，简称`IPC`）的一种高效方式，它**允许多个进程直接访问同一块物理内存区域**，避免了数据复制的开销。

## 共享内存的核心原理

1. **内存映射**
操作系统在内核中分配一块物理内存，多个进程将其映射到各自的虚拟地址空间，这块内存就是`共享内存`。

2. **直接访问**
**进程通过指针直接读写共享内存**，无需系统调用（如 `read`/`write`），速度接近普通内存操作，是`最高效的/最快的IPC方式`。

3. **无结构数据**
共享内存本身只是一块字节流，通常需要自行定义数据格式（如结构体、类、协议头等）。

## 共享内存相关命令

### System V 共享内存

由内核的 IPC 子系统管理，通过 shmget 创建，shmat 映射。

资源信息存储在 /proc/sysvipc/shm 中，由 ipcs -m 解析并显示。

ipcs: 查看共享内存
```bash
ipcs -m
```

ipcrm: 删除共享内存
```bash
ipcrm -m shmid
ipcrm -M shmkey
```

自动清理：系统重启后会清空共享内存。

### POSIX 共享内存

通过挂载在 `/dev/shm` 的 虚拟文件系统（tmpfs）实现，shm_open 本质上创建了一个内存文件。

共享内存对象表现为文件（如 /dev/shm/my_shm），而非传统的 IPC 资源。

OSIX 共享内存对象会以文件形式出现在 `/dev/shm` 目录中。

通过文件系统查看
```shell
ls -l /dev/shm
```
通过 `lsof` 查看占用进程
```shell
lsof /dev/shm/my_shm
```
通过 `fuser` 查看进程
```shell
fuser -v /dev/shm/my_shm
```

`rm` 手动删除
```shell
rm /dev/shm/my_shm  # 直接删除文件（需权限）
```

自动清理：系统重启后 /dev/shm 下的内容会被清空。

## 相关函数 - System V API

### shmget函数

`shmget`函数用于`创建`共享内存 或 `获取`已存在的共享内存。

```cpp
#include <sys/shm.h>
int shmget(key_t key, size_t size, int shmflg);  // 返回共享内存标识符
```
- key: 共享内存标识符或键值，用于标识共享内存，推荐使用`十六进制`（如 0x5005），因为`ipcs -m`命令 显示的key为十六进制，二者一致方便查看。
- size: 共享内存的大小，以字节为单位。
- shmflg: 标志位，用于控制共享内存的创建和访问权限。

return: 成功返回共享内存标识符，失败返回-1。如果key不存在，则创建共享内存，如果key已存在，则获得共享内存。

### shmat函数

`shmat`函数用于将共享内存 `附加`/`连接` 到进程的地址空间。

```cpp
#include <sys/shm.h>
void *shmat(int shmid, const void *shmaddr, int shmflg);  // 返回共享内存地址
```
- shmid: 共享内存标识符，由`shmget`函数返回。
- shmaddr: 指定共享内存连接到进程中的地址，通常设为`NULL`/`nullptr`，让系统自动选择地址。
- shmflg: 标志位，用于控制共享内存的访问权限。

return: 成功返回共享内存地址，失败返回`(void *)-1`。

### shmdt函数

`shmdt`函数用于将共享内存从进程的地址空间`分离`。

```cpp
#include <sys/shm.h>
int shmdt(const void *shmaddr);  // 返回0
```
- shmaddr: 共享内存地址，由`shmat`函数返回。

return: 成功返回0，失败返回-1。

### shmctl函数

`shmctl`函数用于控制共享内存的属性。

```cpp
#include <sys/shm.h>
int shmctl(int shmid, int cmd, struct shmid_ds *buf);  // 返回0
```
- shmid: 共享内存标识符，由`shmget`函数返回。
- cmd: 控制命令，如`IPC_STAT`/`IPC_SET`/`IPC_RMID`等。
- buf: 指向`shmid_ds`结构体的指针，用于获取/设置共享内存的属性。

return: 成功返回0，失败返回-1。

### close函数

`close`函数用于`关闭`共享内存标识符。

```cpp
#include <unistd.h>
int close(int fd);  // 返回0
```
- fd: 共享内存标识符，由`shmget`函数返回。
return: 成功返回0，失败返回-1。

### 实现步骤（System V API）

示例代码，只是一个单进程的简单的共享内存读写操作。还可以将共享内存映射到多个进程，实现进程间通信。

```cpp
#include <iostream>

#include <sys/shm.h>
#include <unistd.h>


int main()
{
    // 创建/获取 共享内存
    int shmid = shmget(0x5005, sizeof(int), IPC_CREAT | 0666);
    if (shmid == -1)
    {
        perror("shmget");
        return 1;
    }
    // 将共享内存附加到进程地址空间
    void* p = shmat(shmid, nullptr, 0);
    if(p == (void*)-1)
    {
        perror("shmat");
        return 1;
    }

    // 读写共享内存
    int* pInt = static_cast<int*>(p);
    *pInt = 777;  // 写入共享内存
    std::cout << "Shared memory value: " << *pInt << std::endl;  // 读取共享内存

    // 分离共享内存
    if(shmdt(p) == -1)
    {
        perror("shmdt");
        return 1;
    }
    // 删除共享内存
    if(shmctl(shmid, IPC_RMID, nullptr) == -1)
    {
        perror("shmctl");
        return 1;
    }
    // 关闭共享内存标识符
    if(close(shmid) == -1)
    {
        perror("close");
        return 1;
    }

    return 0;
}
```

## 相关函数 - POSIX API

### shm_open函数

`shm_open`函数用于`创建`或`打开`共享内存。

```cpp
#include <sys/mman.h>
#include <sys/stat.h>        /* For mode constants */
#include <fcntl.h>           /* For O_* constants */

int shm_open(const char *name, int oflag, mode_t mode);  // 返回共享内存标识符
```
- name: 共享内存名称，用于标识共享内存，推荐使用`十六进制`（如 0x5005），因为`ipcs -m`命令 显示的key为十六进制，二者一致方便查看。
- oflag: 标志位，用于控制共享内存的创建和访问权限，如O_CREAT、O_RDWR等。
- mode: 权限位，用于设置共享内存的权限，如0666。

return: 成功返回 共享内存标识符（文件描述符），失败返回-1。

### ftruncate函数

`ftruncate`函数用于 `设置`/`调整` 共享内存的大小。

```cpp
#include <unistd.h>
int ftruncate(int fd, off_t length);  // 返回0
```
- fd: 共享内存文件描述符，由`shm_open`函数返回。
- length: 共享内存的新大小，以字节为单位。

return: 成功返回0，失败返回-1。

### mmap函数

`mmap`函数用于将共享内存`映射`到进程的地址空间。

```cpp
#include <sys/mman.h>
void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);  // 返回映射后的地址
```
- addr: 指定共享内存映射到进程中的地址，通常设为`NULL`/`nullptr`，让系统自动选择地址。
- length: 共享内存的大小，以字节为单位。
- prot: 保护标志，用于控制共享内存的访问权限。
- flags: 标志位，用于控制共享内存的映射方式，如PROT_READ、PROT_WRITE 等。
- fd: 共享内存文件描述符，由`shm_open`函数返回。
- offset: 共享内存的偏移量，通常设为0。

return: 成功返回映射后的地址，失败返回`MAP_FAILED`即`(void *)-1`。

在mmap（）调用返回后，文件描述符fd可以立即关闭，而不会使映射无效。

### munmap函数

`munmap`函数用于将共享内存从进程的地址空间`分离` / `取消映射`。

```cpp
#include <sys/mman.h>
int munmap(void *addr, size_t length);  // 返回0
```
- addr: 共享内存地址，由`mmap`函数返回。
- length: 共享内存的大小，以字节为单位。

return: 成功返回0，失败返回-1。

### shm_unlink函数

`shm_unlink`函数用于`删除`共享内存。

```cpp
#include <sys/mman.h>        /* For shm_open() and mmap() */
int shm_unlink(const char *name);  // 返回0
```
- name: 共享内存名称，由`shm_open`函数返回。

return: 成功返回0，失败返回-1。

### close函数
`close`函数用于`关闭`共享内存标识符。
```cpp
#include <unistd.h>
int close(int fd);  // 返回0
```
- fd: 共享内存标识符，由`shm_open`函数返回。

return: 成功返回0，失败返回-1。


### 实现步骤（POSIX API）

```cpp
#include <iostream>
#include <unistd.h>
#include <fcntl.h>
#include <sys/mman.h>

#include <cstring>

int main()
{
    // 创建/打开 共享内存
    int shmid = shm_open("test", O_CREAT | O_RDWR, 0666);
    if(shmid == -1)
    {
        perror("shm_open");
        return 1;
    }
    // 设置共享内存大小
    size_t shm_size = 1024;
    if(ftruncate(shmid, shm_size) == -1)
    {
        perror("ftruncate");
        return 1;
    }
    // 映射共享内存到进程地址空间
    void* ptr = mmap(nullptr, shm_size, PROT_READ | PROT_WRITE, MAP_SHARED, shmid, 0);
    if(ptr == MAP_FAILED)
    {
        perror("mmap");
        return 1;
    }
    // close(shmid); // mmap() 映射成功后，就可以关闭共享内存标识符了, 因为共享内存标识符是用于创建共享内存的，而不是用于访问共享内存的。

    // 将 共享内存的 void*无类型指针 转换为 实际数据类型指针
    char* p = static_cast<char*>(ptr);

    // 写入数据到共享内存
    p[0] = 'a';
    *(p) = '-';
    p[1] = 'b';
    p[2] = 'c';
    p[3] = '\0';  // 字符串结束符
    std::cout << p << std::endl;  // 从共享内存读取数据

    strcpy(p, "hello");  // 将字符串复制到共享内存
    std::cout << p << std::endl;  // 从共享内存读取数据

    strncat(p, " world", shm_size-strlen(p)-1);  // 将字符串追加到共享内存，第三个参数为 追加的最大长度，防止越界
    std::cout << p << std::endl;  // 从共享内存读取数据

    strncpy(p, "Hello World!", shm_size);  // 将字符串复制到共享内存，第三个参数为 复制的最大长度，防止越界
    std::cout << p << std::endl;  // 从共享内存读取数据

    // 取消映射共享内存
    if(munmap(ptr, shm_size) == -1)
    {
        perror("munmap");
        return 1;
    }
    // 关闭共享内存标识符
    if(close(shmid) == -1)
    {
        perror("close");
        return 1;
    }
    // 删除共享内存
    if(shm_unlink("test") == -1) // 删除
    {
        perror("shm_unlink");
        return 1;
    }
    return 0;
}
```


## 同步机制（关键！）

**共享内存本身不提供同步**，即在某一个进程对共享内存进行读/写的时候，不会阻止其它进程对它的读/写。

如果有需要，需结合其他 IPC 机制防止竞态条件。由于多个进程共享一段内存，因此需要依靠某种同步机制（如信号量）来达到进程间的同步及互斥。
如果要对共享内存的读/写加锁，可以使用`信号量`。

## 注意

1. 共享内存的生命周期与进程无关，进程退出后共享内存仍然存在。
2. 共享内存的大小应根据实际需求合理设置，过大可能导致内存不足。
3. 共享内存的读写操作需谨慎，避免出现数据不一致或越界等问题。
4. 共享内存的使用需注意进程间的同步和互斥，避免竞态条件。
5. 共享内存的数据不会自动初始化，需要在进程中自行初始化。
6. 共享内存的数据不会被自动清空，需要在进程中自行清空。
7. 共享内存的数据不会被读走，只是取出数据，需要在进程中自行清空，或者覆盖原有数据。
8. 不要使用`C++`的 stl容器，作为向共享内存写入的数据类型。若容器申请了`堆内存`，会将指向该堆内存的指针写入共享内存，该地址是虚拟地址，不同进程的虚拟地址空间不一样，获取不到正确的数据。  
例如`std::string`保存长字符串时，这个长字符串会存储在`堆空间`，而std::string对象位于栈内存，会保存一个指向堆内存的指针，同时记录字符串的长度和容量等数据。将此std::string对象写入共享内存时，实际写入的是它的成员变量（包括指针、长度、容量等），而 **堆内存中的字符串数据本身并不会被复制到共享内存**。
9. 除了 基本数据类型 变量/数组，通常需要自定义数据类型（结构体、类）。

    向共享内存写入字符串，通常需要使用字符数组 或 自定义数据格式，如：
    ```cpp
    const int MaxSize = 4096;

    char shared_buffer[MaxSize];  // 直接使用字符数组

    // 自定义：共享内存结构定义
    struct SharedData
    {
        char message[MaxSize]; // 固定大小数组
        int message_length;   // 显式存储字符串长度（可选）
        // ... 其他数据
    };
    ```
10. 共享内存实现生产者-消费者模式，要使用`循环队列`，避免读写指针越界。使用同步机制（如信号量）来达到进程间的同步及互斥。
