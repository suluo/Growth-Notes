[教程](https://blog.csdn.net/kevin_hx001/article/details/7578835)

student.thrift

```
struct Student{
 1: i32 sno,
 2: string sname,
 3: bool ssex,
 4: i16 sage,
}
service Serv{
 i32 put(1: Student s),
}
```

```
# 生成 server simple
thrift -r --gen c++ student.thrift
thrift -r -strict –gen cpp:cob_style -o ./ student.thrift

# 生成 客户端
thrift -r --gen py student.thrift
```

[demo hello示例](https://blog.csdn.net/tocpp/article/details/6330090)

