tornado 高并发

[线程](https://blog.csdn.net/qq_28893679/article/details/69437496)

[进程 or celery](https://www.cnblogs.com/hepingqingfeng/p/6655790.html)



多线程多进程

进程

[mutilprocess简介](https://www.cnblogs.com/IPYQ/p/5573628.html)

```
# fock
import os

print "Process %s start ..." %(os.getpid())
pid = os.fork()
if pid == 0:
    print "This is child process and my pid is %d, my father process is %d" %(os.getpid(), os.getppid())
else:
    print "This is Fater process, And Its child pid is %d" %(pid)

## 子进程fork的结果总是0， 所以也可以作为区分父进程和子进程的标志

# multiprocessing
from multiprocessing import Process
import os

def pro_do(name, func):
    print "This is child process %d from parent process %d, and name is  %s which is used for %s" %(os.getpid(), os.getppid(), name, func)

if __name__ == "__main__":
    print "Parent process id %d" %(os.getpid())
    #process 对象指定子进程将要执行的操作方法(pro_do), 以及该函数的对象列表args(必须是tuple格式， 且元素与pro_do的参数一一对应)
    pro = Process(target=pro_do, args=("test", "dev"))
    print "start child process"
    #启动子进程
    pro.start()
    #是否阻塞方式执行， 如果有， 则阻塞方式， 否则非阻塞
    pro.join() #if has this, it's synchronous operation or asynchronous operation
    print "Process end"

# 进程类
# 通过继承Process类，来自定义进程类，实现run方法。实例p通过调用p.start()时自动调用run方法。 
import multiprocessing

class Worker(multiprocessing.Process):

    def run(self):
        print 'In %s' % self.name
        return

if __name__ == '__main__':
    jobs = []
    for i in range(5):
        p = Worker()
        jobs.append(p)
        p.start()
    for j in jobs:
        j.join()
```

进程池

[进程池使用](https://www.cnblogs.com/kaituorensheng/p/4465768.html)

```
from multiprocessing import Pool
import os, time

def pro_do(process_num):
    print "child process id is %d" %(os.getpid())
    time.sleep(6 - process_num)
    print "this is process %d" %(process_num)

if __name__ == "__main__":
    print "Current process is %d" %(os.getpid())
    p = Pool(10)   # 默认4
    for i in range(25):
        p.apply_async(pro_do, (i,))  #增加新的进程
    p.close() # 禁止在增加新的进程
    p.join()  # 等待所有进程结束
    print "pool process done"
```

进程通信

```
# queue

from multiprocessing import Process, Queue
import os, time

def write_queue(q):
    for name in ["Yi_Zhi_Yu", "Tony" ,"San"]:
        print "put name %s to queue" %(name)
        q.put(name)
        time.sleep(2)
    print "write data finished"

def read_queue(q):
    print "begin to read data"
    while True:
        name = q.get()
        print "get name %s from queue" %(name)

if __name__ == "__main__":
    q = Queue()
    pw = Process(target=write_queue, args=(q,))
    pr = Process(target=read_queue,args=(q,))

    pw.start()
    pr.start()
    pw.join() #这个表示是否阻塞方式启动进程， 如果要立即读取的话， 两个进程的启动就应该是非阻塞式的， 所以pw在start后不能立即使用pw.join(), 要等pr start后方可
    pr.terminate() #服务进程，强制停止


# pipe 两个进程通信

from multiprocessing import Process,Pipe
import os,time,sys

def send_pipe(p):
    names = ["Yi_Zhi_Yu", "Tony", "San"]
    for name in names:
        print "put name %s to Pipe" %(name)
        p.send(name)
        time.sleep(1)
def recv_pipe(p):
    print "Try to read data in pipe"
    while True:
            name = p.recv()
            print "get name %s from pipe" %(name)

if __name__ == "__main__":
   #pipe, one for send, one for read
   ps_pipe, pr_pipe = Pipe()
   #process
   ps = Process(target=send_pipe, args=(ps_pipe,))
   pr = Process(target=recv_pipe, args=(pr_pipe,))
   pr.start()
   ps.start()
   ps.join()
   pr.terminate()
```



