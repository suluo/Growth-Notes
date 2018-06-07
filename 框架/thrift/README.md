[libevent](http://www.cnblogs.com/linjiqin/p/6943927.html) 安装

```
yum install automake libtool flex bison pkgconfig gcc-c++ boost-devel libevent-devel zlib-devel python-devel ruby-devel
```

```
wget https://github.com/downloads/libevent/libevent/libevent-2.0.21-stable.tar.gz
tar zxvf libevent-2.0.21-stable.tar.gz
cd libevent-2.0.21-stable 
./configure -prefix=/usr
make 
sudo make install


$ ls -al /usr/lib | grep libevent 
```

[thrift 源码安装](http://thrift.apache.org/lib/cpp)

另外：[ boost.python](https://blog.csdn.net/ccat/article/details/534748)

