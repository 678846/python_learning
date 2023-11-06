## 并发编程

```te
并发编程：通过代码编程让计算机在一定时间内运行多个程序的操作(充分利用cpu,提高处理性能)



串行、并行、并发
	串行：一个任务结束后才会运行其任务
	并行：同一时刻内运行多个任务
	并发：同一时间内运行多个任务

同步和异步
	同步：一个任务完成后才执行下一个任务
	异步：同时执行多个任务，不需要等待任务的结果，可以去执行其他任务

阻塞和非阻塞
	阻塞：执行一个任务不能立即得到结果，需要等待，cpu不能执行其他任务。(cpu不工作了)
	非阻塞：执行一个任务不能立即得到结果，需要等待，但是cpu可以去执行其他任务。(cpu工作中)

```



## 进程

```tex
概念：是操作系统进行资源分配和调度的基本单位
进程的标记
	1.操作系统运行一个程序就会创建一个进程id(即pid)，是操作系统用来区分进程的唯一标识。
	2.进程运行时固定不变，进程执行结束时，操作系统会回收进程的一切(包括pid)
进程的调度
	1.FCFS调度算法 先来先服务调度算法
	2.SJF/SPF调度算法 短作业优先调度算法
	3.RR调度算法 时间片轮转调度算法	
	4.MFQ调度算法 多级反馈队列调度算法，目前公认最优、最公平的作业调度算法
```

```python
进程的状态
	就绪：进程分配到处cpu以外的所有资源，只要获得cpu资源就立即执行
    执行：进程获得cpu资源，程序在cpu上执行
    阻塞：正在执行的进程由于发生某事件（如IO请求、申请缓冲区失败等）暂时无法继续执行的状态
所有程序想要执行就必须经历就绪状态
#程序开始运行之后不会立即执行代码，而是处于就绪状态等待系统调度开始运行
import time
print("程序开始运行")  #程序运行状态
#等待用户输入，程序进入阻塞状态
name=input('-->')
#用户输入之后，进程并不会立即执行，而是进入就绪状态，等待操作系统调度运行
print(name)  #运行
time.sleep(1)   #阻塞
print("程序运行结束")     #运行
#结束
```

![image-20231023154046019](C:\Users\user-liu\AppData\Roaming\Typora\typora-user-images\image-20231023154046019.png)
![image-20231023154109529](C:\Users\user-liu\AppData\Roaming\Typora\typora-user-images\image-20231023154109529.png)



## 线程

```python
概念：包含在进程内，进程中实际运作的单位，cpu进行任务调度的最小单位
进程和线程之间的关系
	1.进程负责资源的分配和隔离，而线程负责具体任务的执行
	2.一个线程只属于一个进程，一个进程可以拥有多个线程。
    3.线程依附于进程，线程没有系统资源，同一个进程下多个线程共享资源
    4.线程的创建，管理，切换，回收也要消耗计算机资源，不过比进程要小的多
    
```



## 协程

```python
1.协程不是计算机提供的，而是程序员人为创造的
2.用户态的上下文切换技术，调度是人控制的，通过一个线程实现代码块相互切换执行的过程。

意义：协程遇到io等待，不会傻傻的等，会利用空闲时间去干点其他的事。
```



# python 相关操作

## **进程**

**os模块**

```python
os模块
	os.getpid()查看当前进程的pid
    os.getppid()查看当前进程父进程的pid

multiprocessing模块
windows系统相面创建子进程的方式：是import导入父进程的代码到子进程，所以要把创建子进程的代码放到 if __name__=='__main__': 下面！

```



**创建子进程**

```python
创建子进程
1.基于multiprocessing.Process创建子进程
import multiprocessing, time,os

def func(name):
    time.sleep(1)
    print(f'{name}正在看电视,子进程pid={os.getpid()}')

if __name__ == "__main__":
    print(os.getpid())
    for i in range(3):
        name = f'子进程{i}号'
        p = multiprocessing.Process(target=func, args=(name,))     #创建子进程
        p.start()

2.继承Process类创建子进程
import os
from multiprocessing import Process
class MyProcess(Process):
    def __init__(self, name):
        super(MyProcess, self).__init__()
        self.name = name

    def run(self):
        print(f'{self.name}子进程运行了，进程pid是{os.getpid()},父进程是{os.getppid()}')

if __name__ == "__main__":
    print('当前进程pid', os.getpid())
    p1 = MyProcess('1号')
    p2 = MyProcess('2号')
    p3 = MyProcess('3号')
    p1.start()  # start() 会自动调用run
    p2.start()
    p3.start()  

```



**join()方法**

```python
join():阻塞等待所有子进程执行结束
import time ,os
from multiprocessing import Process
def func():
    time.sleep(1)
    print(f'我是子进程{os.getpid()},父进程是{os.getppid()}')

if __name__=='__main__':
    p=Process(target=func)
    p.start()
    p.join()  #主进程交出cpu使用权，等待阻塞所以子进程执行结束
    print(f'我是主进程{os.getpid()}')
    #我是子进程10068,父进程是16592
	#我是主进程16592
```



**守护进程**

```python
主进程创建守护进程后，守护进程作为一个特殊的子进程伴随着主进程的结束而结束
import time
from multiprocessing import Process
def func():
    time.sleep(2)
    print('我是子进程，我正在执行')
#守护进程，会伴随主进程结束而结束
if __name__=="__main__":
    p=Process(target=func)
    p.daemon=True   #设置子进程为守护进程，必须在start()之前
    p.start()
    print('我是主进程')
    #我是主进程，子进程没执行完就伴随着主进程的结束而结束了
```



**current_process**

```python
current_process 读取当前进程操作对象
from multiprocessing import Process, current_process

def func():
    p = current_process()
    print(f'当前进程名{p.name}')
    print(f'当前进程pid{p.pid}')


if __name__ == '__main__':
    p = Process(target=func, name='xiaoliu')
    p.start()
    print('主进程执行结束')
```



**进程锁**

```python
Lock解决并发对共享数据的操作导致的数据不一致问题
import json, time
from multiprocessing import Process, Lock

# 多进程同时打开操作同一个文件，就会出现资源冲突导致的报错问题。
# 这里体现的就是多进程并发/并行情况下会带来数据操作的过程中一致性问题。（表现在商品超卖，转账结果出错等情况）


def get_ticket(username):
    """查询余票"""
    time.sleep(0.01)
    f=open("ticket.txt")
    data = json.loads(f.read())
    f.close()

    print(f"{username}查询余票：{data['count']}")


def buy_ticket(username):
    """购买车票"""
    time.sleep(0.1)
    f=open('ticket.txt')
    data = json.loads(f.read())
    f.close()

    # 判断如果有票，则购买
    if data["count"] > 0:
        data["count"] -= 1  # 买票
        print(f"{username}购买车票成功!")
    else:
        print(f"{username}购买车票失败!")
    time.sleep(0.1)
    json.dump(data, open("ticket.txt", "w"))


def task(username, lock):
    """购票流程"""
    get_ticket(username)
    # 给需要加锁的代码设置加锁（拿钥匙）和解锁（换钥匙）操作
    lock.acquire()  # 加锁
    buy_ticket(username)  # acquire与release就属于被加锁的代码，它是基于同步阻塞方式运行的
    lock.release()  # 解锁
    # with lock:                #建议使用with lock，即使加锁的代码发生错误也能解锁
    #     buy_ticket(username)

if __name__ == '__main__':
    # 在进程中，创建锁
    lock = Lock()
    for i in range(10):
        # 把锁传递到需要加锁的每个子进程中
        p = Process(target=task, args=(f"user-{i}", lock))
        p.start()
```



**进程之间数据的隔离**

```python
import time
from multiprocessing import Process
num=100
def func():
    global num
    time.sleep(2)
    num-=1
    print(num)

if __name__=='__main__':
    p_list=[]
    for i in range(10):
        p=Process(target=func)
        p.start()
        # p.join()    #同步阻塞
    for p in p_list:
        p.join()
        # p.start()
    print(num) 	#100
```



**进程之间的通信**

```python
from multiprocessing import Process,Queue
def func(str1,p):
    ret=eval(str1)
    p.put(ret)
    #将数据放到队列中如果队列已满等待另一个进程取出数据

if __name__=='__main__':
    q=Queue()#创建队列对象 先进先出
    str1='30+20+1'
    p=Process(target=func,args=(str1,q))
    p.start()
    print(q.get())
    #将数据从队列中取出如果队列为空等待另一个进程放入数据
```



**进程的缺点**

```python
操作系统在每个进程上的创建，管理，切换，回收灯等操作上会消耗一部分计算机资源。如果在多任务，多进程场景下，cpu会消耗大量的资源浪费在进程的创建和回收等操作上。
```





## 线程

**线程的创建**

```python
1.Thread创建
import threading
import time

def func(name):
    time.sleep(2)
    print(f'{name}子线程正在运行！')

if __name__ == '__main__':
    t1 = threading.Thread(target=func, args=('1号',))   #异步非阻塞
    t2 = threading.Thread(target=func, kwargs={'name':'2号'})
    t1.start()
    t2.start()
    
    
2.基于Thread类创建
from threading import Thread

class MyThread(Thread):
    def __init__(self,name):
        super(MyThread, self).__init__()
        self.name=name
    def run(self):
        print(f"{self.name}子线程正在运行！")

if __name__=='__main__':
    t1=MyThread('1号')
    t2=MyThread('2号')
    t1.start()
    t2.start()
```



**多线程共享资源**

```python
from threading import Thread
import time
num=10
def func():
    time.sleep(2)
    global num
    num-=1

if __name__=='__main__':
    t_li=[]
    start=time.time()
    for i in range(10):
        t=Thread(target=func)
        t_li.append(t)
        t.start()
    for t in t_li:
        t.join()


    print(num,time.time()-start)#0
```



**多进程多线程的创建**

```python
from multiprocessing import Process
from threading import Thread


def process(name):
    for i in range(3):
        t = Thread(target=thread, args=(name, {i},))
        t.start()


def thread(name, i):
    print(f'{name}.{i}子线程开始运行')


if __name__=='__main__':
    for i in range(3):
        p=Process(target=process,args=(i,))
        p.start()
```



**join（）方法**

```python
from threading import Thread
import time
num=10
def func():
    time.sleep(2)
    global num
    num-=1

if __name__=='__main__':
    t_li=[]
    start=time.time()
    for i in range(10):
        t=Thread(target=func)
        t_li.append(t)
        t.start()
    for t in t_li:
        t.join()#阻塞等待子线程执行结束


    print(num,time.time()-start)#0
```



**current Thread**

```python
currentThread :获取当前线程对象
import time
from threading import Thread, current_thread, enumerate, active_count


def func():
    time.sleep(2)
    # 获取当前线程对象
    t = current_thread()
    print(f'子线程{t.name}运行了')


if __name__ == "__main__":
    for i in range(5):
        t = Thread(target=func, name=f'{i}号')
        t.start()

    print(enumerate())
    print(active_count())
```



**其他方法**

```python
enumerate  获取当前进程的所有线程对象组成的列表
active_count  获取当前进程下线程的个数
```



**GIL（全局解释器锁）**

```python
Cpython解释器提供的全局锁，GIl会限制每个线程执行前都会获取GIL锁，保证了运行多个线程时，同一时间只有一个线程被cpu执行

特点：能发挥并发效果，不能发挥多核cpu性能

数据一致问题
from threading import Thread
num = 0
def func():
    global num
    # 大批量执行计算操作,一般叫计算密集型的任务
    for i in range(200000):
        num += 1

if __name__ == '__main__':
    thread_list = []
    for i in range(10):
        t = Thread(target=func)
        t.start()
        thread_list.append(t)

    for t in thread_list:
        t.join()

    print(num) # num=？ 2000000?  1356056

原因：GIL与时间片轮转法之间的冲突，pyhon中赋值操作时容易产生数据不一致问题，计算密集型任务可能会产生计算结果，当时间片到了，cpu会切换到其他任务，下次切换回来，就会丢失上次计算的结果

适合多线程网络爬虫，对io密集任务影响较小
```



**合理加锁**

```python
from threading import Thread, Lock
num = 0

def func(lock):
    global num

    # for i in range(200000):
    #     lock.acquire()    #不推荐，频繁的加锁解锁会浪费大量的cpu资源
    #     num += 1
    #     lock.release()
    with lock:
        for i in range(200000):
            num+=1

if __name__ == '__main__':
    thread_list = []
    lock=Lock()
    for i in range(10):
        t = Thread(target=func,args=(lock,))
        t.start()
        thread_list.append(t)

    for t in thread_list:
        t.join()

    print(num)  
```



## 队列

```python
1.先进先出的一种数据结构   删除数据项在头部，添加数据项在尾部
from multiprocessing import Queue #进程的队列
import queue    #另一个队列模块
# 初始化队列对象，可以限制队列对象的长度，默认不限制队列对象长度
import pylab as p

q=queue.Queue(3)
#如果队列为空返回True
print(q.empty())
q.put('a')
q.put('b')
q.put('c')
#如果队列满了，再次使用put添加数据项，就会进入阻塞状态等待下一个进程从队列中提取数据项
#put 与 get一一对应
print(q.get())
print(q.get())
print(q.get())
#如果队列为空，再次使用get就会进入阻塞状态，直到另一个进程向队列中添加数据项
print(q.empty())
场景：排队

2.先进后出
import queue

# 初始化队列对象，可以指定限制队列的长度，如果不设置长度，则默认队列的长度没有上限
q = queue.LifoQueue(3)

# 进队
q.put(1)
q.put(2)
q.put(3)
# q.put(4)  #  队列如果设置长度，则put的次数不能连续超过队列长度，否则会阻塞，甚至会报错
# print(q.empty())  # False 表示对队列有成员，不为空
# print(q.qsize())  # 获取队列的长度

# 出队
print(q.get())
print(q.get())
print(q.get())

# print(q.get())  # 队列如果设置长度，则get的次数不能连续超过队列长度，否则也会阻塞么，设置会报错

# print(q.empty())  # False 表示对队列有成员，不为空
# print(q.qsize())  # 获取队列的长度
场景：浏览器历史记录

3.优先级队列
import queue

# 初始化队列对象，可以指定限制队列的长度，则默认队列的长度没有上限
q = queue.PriorityQueue(5)

# 进队
# 成员是一个元组(优先级,成员值)
q.put((1, "a"))
q.put((2, "b"))
q.put((3, "c"))
q.put((1, "d"))
q.put((10, "e"))
# 超出队列的范围，也会阻塞

print(q.empty()) # False，表示队列有成员
print(q.qsize()) 

# 出队
# 返回的成员也是一个元组(优先级,成员值)
print(q.get())  
print(q.get())  
print(q.get())  
print(q.get())  
print(q.get())  
# print(q.get())  # 对空列表取值，会阻塞，甚至报错

print(q.empty()) # True，表示空队列，没有成员
print(q.qsize())
```

