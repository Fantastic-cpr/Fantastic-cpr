# Netty学习

[toc]

## 介绍

Netty是一种可以轻松快速的开发协议服务器和客户端网络应用程序的NIO框架，它大大简化了TCP或者UDP服务器的网络编程,但是你仍然可以访问和使用底层的API,Netty只是对其进行了高层的抽象。

Netty的简易和快速开发并不意味着由它开发的程序将失去可维护性或者存在性能问题。Netty是被精心设计的，它的设计参考了许多协议的实现，比如FTP，SMTP，HTTP和各种二进制和基于文本的传统协议，因此 Netty成功的实现了兼顾快速开发，性能，稳定性，灵活性为一体，不需要为了考虑一方面原因而妥协其他方面。

## 概念

### 在Netty中

* 客户端持有一个EventLoopGroup用来处理网络IO操作；
* 服务器端持有两个EventLoopGroup；
* boss组 是专门接受客户端发来的TCP连接请求的；
* worker组 专门用来具体处理完成三次握手的连接套接字的网络IO请求的；

### NIO介绍

NIO（Non-blocking I/O，在Java领域，也称为New I/O），是一种同步非阻塞的I/O模型，也是I/O多路复用的基础，已经被越来越多地应用到大型应用服务器，成为解决高并发与大量连接、I/O处理问题的有效方式。

NIO一个重要的特点是：socket主要的读、写、注册和接收函数，在等待就绪阶段都是非阻塞的，真正的I/O操作是同步阻塞的（消耗CPU但性能非常高）。

### TCP三次握手

[网络编程之TCP三次握手与四次挥手、基于TCP协议的套接字编程](https://www.cnblogs.com/Hades123/p/11099346.html#tcp三次握手和四次挥手)

## Netty服务器端开发步骤

​	首先是服务器端，exam_systemServer类，用于构建Netty相关的组件，并进行初始化；使用Netty进行服务器端开发主要有以下几个步骤：

#### 1、创建ServerBootstrap实例

```java
ServerBootstrap b=new ServerBootstrap();
```

​	ServerBootstrap是Netty服务器端的启动辅助类，提供了一系列的方法用于设置服务器端启动相关的参数。

#### 2、设置并绑定Reactor线程池

```java
EventLoopGroup bossGruop=new NioEventLoopGroup();//用于服务器端接受客户端的连接
EventLoopGroup workGroup=new NioEventLoopGroup();//用于网络事件的处理
```

​	Netty的线程池是EventLoopGroup，它实际上是EventLoop的数组，EventLoop职责是处理所有注册到本线程多路复用器Selector上的Channel，Selector的轮询操作是由绑定的EventLoop线程run方法驱动。

#### 3、设置并绑定服务器端Channel

```java
b.group(bossGruop, workGroup).channel(NioServerSocketChannel.class)
```

​	Netty对原生的NIO类库进行封装，作为NIO服务端，需要创建ServerSocketChannel，对应的实现是NioServerSocketChannel。

#### 4、链路建立的时候创建并初始化ChannelPipeline

```java
b.group(bossGruop, workGroup).channel(NioServerSocketChannel.class).childHandler(new ChannelInitializer<SocketChannel>()
```

​	ChannelPipeline的本质是一个负责处理网络事件的职责链，负责管理和执行ChannelHandler。网络事件以事件流的形式在ChannelPipeline中流转，由ChannelPipeline根据Channel41Handler的执行策略调度ChannelHandler的执行。典型的网络事件有：

- 链路注册
- 链路激活
- 链路断开
- 接收到请求信息
- 请求信息接收并处理完毕
- 发送应答消息
- 链路发生异常
- 用户自定义事件

#### 　5、添加并设置ChannelHandler

```java
b.group(bossGroup, workerGroup)//设置时间循环对象，
 .channel(NioServerSocketChannel.class)//用它来建立新accept的连接，用于构造serversocketchannel的工厂类 
 .childHandler
 (
    new ChannelInitializer<SocketChannel>() 
  { 
      @Override//当新连接accept的时候，这个方法会调用
      public void initChannel(SocketChannel ch) throws Exception 
      {
        //设置发送时的编码器，4表示长度占四个字节
        ch.pipeline().addLast(new LengthFieldPrepender(4, false));
        //接收时的解码器，参数依次为： 每帧最大字节数、长度字节偏移量、长度字节数、长度调整值、返回数据字节偏移量
        ch.pipeline().addLast(new LengthFieldBasedFrameDecoder(1024*1024,0,4,0,4));
        //自定义的处理器，为当前的channel的pipeline添加自定义的处理函数 
          ch.pipeline().addLast(new exam_systemServerHandler());
      }
  }
 )
 //配置tcp参数，将BACKLOG设置为128
 .option(ChannelOption.SO_BACKLOG, 128)          
 .childOption(ChannelOption.SO_KEEPALIVE, true);
```

ChannelHandler是Netty提供给用户定制和扩展的接口，例如消息编解码、心跳、安全认证、TSL/SSL认证。

#### 6、绑定并启动监听窗口	

```java
ChannelFuture f = b.bind(port).sync(); 
```

经过一系列初始化和检测工作后，会启动监听端口，并将ServerSocketChannel注册到Selector上监听客户端连接。



## 客户端开发

### 编写exam_systemServerHandler类：

```java
public class exam_systemServerHandler extends ChannelInboundHandlerAdapter
{
    //...具体代码
}
```

完成以上步骤后实现客户端发送请求到服务器端，服务器端解析客户端请求并返回响应