main

```cpp
#include <string>  
#include <thrift/Thrift.h>  
#include <thrift/protocol/TProtocol.h>  
#include <thrift/transport/TSocket.h>  
#include <thrift/transport/TTransportUtils.h>  
#include <thrift/concurrency/ThreadManager.h>  
#include <thrift/transport/TBufferTransports.h>  
#include <thrift/server/TServer.h>  
#include <thrift/async/TAsyncChannel.h>  
#include <thrift/async/TEvhttpClientChannel.h>  
#include "common/thrift/Twitter.h"  
#include <thrift/async/TAsyncProtocolProcessor.h>  
#include <thrift/async/TEvhttpServer.h>  

using namespace apache::thrift;  
using namespace apache::thrift::protocol;  
using namespace apache::thrift::transport; 
using std::string;
using namespace example;  
using namespace apache::thrift::async;  
using namespace apache::thrift::stdcxx;


class TwitterAsyncHandler : public TwitterCobSvIf {  
 public:  
  TwitterAsyncHandler() {  
    // Your initialization goes here  
  }  


  void sendString(std::function<void(std::string const& _return)> cob, const std::string& data) {  
    printf("sendString rec:%s\n", data.c_str());  // 输出收到的数据  
    std::string _return = "world";   // 返回world字符串给客户端  
    return cob(_return);  
  }  


};  


int main(int argc, char **argv) {
    printf("start.../n");

    shared_ptr<TAsyncProcessor> underlying_pro(new TwitterAsyncProcessor( shared_ptr<TwitterCobSvIf>(new TwitterAsyncHandler()) ) );  
    shared_ptr<TAsyncBufferProcessor> processor( new TAsyncProtocolProcessor( underlying_pro, shared_ptr<TProtocolFactory>(new TBinaryProtocolFactory()) ) );  

    TEvhttpServer server(processor, 14488);  
    server.serve();  


    printf("end/n");
    return 0;
}
```

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



