头

```cpp
// 原有
#include "Serv.h"  // 替换成你的.h  
#include <thrift/transport/TServerSocket.h>  
#include <thrift/transport/TBufferTransports.h>  
#include <thrift/protocol/TBinaryProtocol.h>  
#include <thrift/server/TSimpleServer.h>
// thread *.h
#include <thrift/concurrency/ThreadManager.ho>
#include <thrift/concurrency/PosixThreadFactory.h>

#include <thrift/server/TThreadPoolServer.h>
#include <thrift/server/TThreadedServer.h>
// 非阻塞
#include <thrift/server/TNonblockingServer.h> //zml

using namespace apache::thrift;  
using namespace apache::thrift::protocol;  
using namespace apache::thrift::transport;
using namespace apache::thrift::server;
using namespace apache::thrift::concurrency;
using namespace apache::thrift::stdcxx;
```

main

```cpp
int main(int argc, char **argv) {
    // thread pool
    shared_ptr<ServHandler> handler(new ServHandler());
    shared_ptr<TProcessor> processor(new ServProcessor(handler));
    shared_ptr<TProtocolFactory> protocolFactory(new TBinaryProtocolFactory());
    //shared_ptr<TTransportFactory> transportFactory(new TBufferedTransportFactory());
    shared_ptr<TTransportFactory> transportFactory(new TFramedTransportFactory());
    //shared_ptr<TTransportFactory> transportFactory(new THttpServerTransportFactory());
    shared_ptr<TServerTransport> serverTransport(new TServerSocket(9090));


    // 指定15个线程
    shared_ptr<ThreadManager> threadManager = ThreadManager::newSimpleThreadManager(15);
    shared_ptr<PosixThreadFactory> threadFactory = shared_ptr<PosixThreadFactory>(new PosixThreadFactory());

    threadManager->threadFactory(threadFactory);
    threadManager->start();
    printf("start.../n");

    // 简单服务
    //TSimpleServer server(processor, serverTransport, transportFactory, protocolFactory);

    // 多线程服务
    TThreadPoolServer server(processor, serverTransport, transportFactory, protocolFactory, threadManager);

    shared_ptr<TServerTransport> serverTransport(new TNonblockingServerSocket(port))
    TNonblockingServer server(processor, protocolFactory, serverTransport, threadManager);
    server.setTaskExpireTime(expire_time);

    server.serve();

    printf("end/n");
    return 0;
}
```

[thrift 服务效率](https://www.cnblogs.com/taoxinrui/p/6388052.html)

client py

```
thrift -r -gen py demo.thrift
# gen-py

import sys
reload(sys)
sys.path.append("./gen-py")

from demo import Demo
from demo.ttypes import *
from thrift import Thrift
from thrift.transport
transport = TSocket.TSorket(ip, port)
```



