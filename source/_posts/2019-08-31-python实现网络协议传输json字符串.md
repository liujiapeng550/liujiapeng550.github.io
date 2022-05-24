---
layout:     post
title:      python实现网络协议传输json字符串
subtitle:   
date:       2019-01-30
author:     JPeng
header-img: img/python.png
catalog: true
tags:
    - python
    - 脚本
    - TCP
    - Json
---



![image](https://github.com/liujiapeng550/liujiapeng550.github.io/blob/master/img/pythonTcp.jpg?raw=true)
# 1.python TCP 代码
client:

```

import socket
#创建一个客户端的socket对象
client = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
 
#设置服务端的ip地址
host = "10.36.135.90"
#设置端口
port = 9092
#连接服务端
client.connect((host, port))
 
#while循环是为了保证能持续进行对话
while True:
    #输入发送的消息
    sendmsg = input("请输入:")
    #如果客户端输入的是q，则停止对话并且退出程序
    if sendmsg=='q':
        break
 
    sendmsg = sendmsg
    #发送数据，以二进制的形式发送数据，所以需要进行编码
    client.send(sendmsg.encode("utf-8"))
    msg = client.recv(1024)
    #接收服务端返回的数据，需要解码
    print(msg.decode("utf-8"))
#关闭客户端
client.close()
```

 ## Server

```

import socket
#创建服务端的socket对象socketserver
socketserver = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
host = '10.36.135.90'
port = 9092
#绑定地址（包括ip地址会端口号）
socketserver.bind((host, port))
#设置监听
socketserver.listen(5)
#等待客户端的连接
#注意：accept()函数会返回一个元组
#元素1为客户端的socket对象，元素2为客户端的地址(ip地址，端口号)
clientsocket,addr = socketserver.accept()
 
#while循环是为了能让对话一直进行，直到客户端输入q
while True:
 
    #接收客户端的请求
    recvmsg = clientsocket.recv(1024)
    #把接收到的数据进行解码
    strData = recvmsg.decode("utf-8")
    #判断客户端是否发送q，是就退出此次对话
    if strData=='q':
        break
    print("收到:"+strData)
    msg = input("回复:")
    #对要发送的数据进行编码
    clientsocket.send(msg.encode("utf-8"))
 

```
# 2.python 中json应用
 1. dumps
将python中的 字典 转换为 字符串

```
test_dict = {'bigberg': [7600, {1: [['iPhone', 6300], ['Bike', 800], ['shirt', 300]]}]}
 #dumps 将数据转换成字符串
 json_str = json.dumps(test_dict)
```
 2. dump
dump: 将数据写入json文件中

```
with open("../config/record.json","w") as f:
     json.dump(new_dict,f)
     print("加载入文件完成...")
```
 3. loads
将 字符串 转换为 字典

```
new_dict = json.loads(json_str)
print(new_dict)
print(type(new_dict))
```

 4. load
把文件打开，并把字符串变换为数据类型

```
with open("../config/record.json",'r') as load_f:
    load_dict = json.load(load_f)
    print(load_dict)
load_dict['smallberg'] = [8200,{1:[['Python',81],['shirt',300]]}]
print(load_dict)

with open("../config/record.json","w") as dump_f:
    json.dump(load_dict,dump_f)
```

# 3.Python 字典(Dictionary)
     1. 什么是字典？
- 字典(dictionary)是除列表以外python之中最灵活的内置数据结构类型。列表是有序的对象集合，字典是无序的对象集合。
- 两者之间的区别在于：字典当中的元素是通过键来存取的，而不是通过偏移存取。
- 字典用"{ }"标识。字典由索引(key)和它对应的值value组成。

```
dict = {}
dict['one'] = "This is one"
dict[2] = "This is two"
 
tinydict = {'name': 'john','code':6734, 'dept': 'sales'}
 
 
print(dict['one'])          # 输出键为'one' 的值
print(dict[2])              # 输出键为 2 的值
print(tinydict)            # 输出完整的字典
print(tinydict.keys())      # 输出所有键
print(tinydict.values())    # 输出所有值
```


    2、用方括号访问字典里的值：

```
dict = {'Name': 'Zara', 'Age': 7, 'Class': 'First'};
 
print("dict['Name']: ", dict['Name'])
print(dict['Age']: ", dict['Age'])

```
