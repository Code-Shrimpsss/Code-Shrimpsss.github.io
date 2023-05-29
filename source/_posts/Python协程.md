---
title: Python协程
date: 2021-06-12 00:00:00
type:
description: 'Python协程简述'
tag: Python
cover: https://s3.bmp.ovh/imgs/2021/12/b722a7e57d58fe30.png
---

# 协程 #

------

**协程是python个中另外一种实现多任务的方式**。在一个线程中的某个函数，可以在任何地方保存当前函数的一些临时变量等信息，然后切换到另外一个函数中执行，注意不是通过调用函数的方式做到的，并且切换的次数以及什么时候再切换到原来的函数都由开发者自己确定

#### 简单实现协程 ####

```python
import time

def work1():
    while True:
        print('--111--')
        yield
        time.sleep(3)
        
def work2():
    while True:
        print('--222--')
        yield
        time.sleep(3)
        
def main():
    w1 = work1()
    w2 = work2()
    while True:
        next(w1)
        next(w2)

if __name__ == '__main__':
    main()
```

