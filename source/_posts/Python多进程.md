---
title: Python多任务多进程
date: 2021-06-10 00:00:00
type:
description: 'Python多任务多进程'
tag: Python
cover: https://s3.bmp.ovh/imgs/2021/12/b722a7e57d58fe30.png
---

###  多任务

**多任务**处理是指用户可以在同一时间内运行多个应用程序,每个应用程序被称作一个任务.

Linux、windows就是支持多任务的操作系统,比起单任务系统它的功能增强了许多。

##### 多任务的执行方式 #####

- 并发：在一段时间内**交替**去执行任务。
- 并行：指的是任务数小于等于cpu核数，即任务真的是一起执行的

### 多进程 ###

#### 进程的概念 ####

一个正在运行的程序或者软件就是一个进程，**它是操作系统进行资源分配的基本单位**。

**一个程序运行后至少有一个进程，一个进程默认有一个线程**，进程里面可以创建多个线程，**线程是依附在进程里面的，没有进程就没有线程**。

#### 进程的使用 ####

Python在 multiprocessing 模块下提供了Process老创建新进程；与Thread类似

##### 使用Process进程也要两种方式： #####

1. 以指定函数作为target， 创建Process对象即可创建新进程
2. 继承Process类，重写它的run() 方法来创建进程类，程序创建Process子类的实例作为进程

- **Process进程类的说明**

  **语法参数:** Process( [group [, target [, name [, args [, kwargs ]]]])

  - group：指定进程组，目前只能使用None
  - target：执行的目标任务名
  - name：进程名字
  - args：以元组方式给执行任务传参
  - kwargs：以字典方式给执行任务传参

  **Process创建的实例对象的常用方法:**

  - start()：启动子进程实例（创建子进程）
  - run()：重写该方法可实现进程的执行体
  - join()：等待子进程执行结束 
  - is_alive()：判断进程是否还活着 （是否在执行）
  - terminate()：不管任务是否完成，立即终止子进程（中断该进程）

  **Process创建的实例对象的常用属性:**

  - name：该属性用于设置或访问进程的名字。（默认为Process-N）

  - daemon：该属性用于判断或设置后台状态。

  - authkey：返回进程的授权key

##### 具体使用 #####

> 1.导入进程包
>
> import multiprocessing
>
> 2.创建子进程并指定执行的任务
>
> sub_process = multiprocessing.Process(target = 任务名)
>
> 3.启动进程执行任务
>
> sub_process.start()

##### 代码示例 #####

```python
import time   # 导入时间包
import multiprocessing  # 导入进程包

def dance():
    while True:
        print('跳舞')
        time.sleep(1) # 延迟1秒输出
    
def song():
    while True:
        print('唱歌')
        time.sleep(1) # 延迟1秒输出

# 一边唱歌一边跳舞~~
def mian():
    # 创建跳舞进程
    process_dance = multiprocessing.Process(target=dance)
    # 开启跳舞进程
    process_dance.start()
    
    # 创建唱歌进程
    process_song = multiprocessing.Process(target=song)
    # 开启唱歌进程
    process_song.start()
 	
# 当模块被直接运行时，以下代码块将被运行; 当模块是被导入时，代码块不被运行。
if __name__ = '__main__':
    main()
```

#### 获取进程编号(PID) ####

获取进程编号的目的是验证主进程和子进程的关系，可以得知子进程是由那个主进程创建出来的。

获取进程编号的两种操作

- os.getpid() 表示获取当前进程编号

- os.getppid() 表示获取当前父进程编号

本文参考文献：

Process:https://www.cnblogs.com/jzxs/p/11428467.html