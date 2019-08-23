### Servlet

## Servlet的生命周期

转自（https://www.cnblogs.com/lixuwu/p/5676164.html）

# Servlet的初始化，运行 销毁的全部过程

# Servlet概述
```text
Sevelet是建立在java多线程机制上的，它的生命周期由`web容器`负责。

当客户端第一次请求某个Servlet时，Servlet容器将会根据web.xml的配置文件实例化这个Servlet类。
当有新的客户端请求该Servlet时，一般不会再实例化该Servlet。

当有多个请求时，Servlet容器会起多个线程来访问`同一个Servlet`实例的service()方法，如果该Servlet实例中有共享的实例变量，
需要注意多线程安全问题。
```
# Servlet的生命周期
Servlet的生命周期定义了Servlet从创建到毁灭的整个过程，如下
```text
1 调用init（）方法初始化
2 调用service（）方法来处理客户的请求
3 调用destroy（）来释放资源，标记自身为可回收
4 被垃圾回收器回收
```
init（）方法
 ```text
 init被设计成只调用一次的方法。他在第一次创建Servlet的时候被调用，用于Servlet的初始化。
 初始化的数据可以在整个生命周期中使用。
 ```
 `初始化时刻`
 ```text
 在下列时刻Servlet容器装载Servlet。一共三种情况：
 1 Servlet容器启动器自动装载某些Servlet，实现它只需要在web.xml文件中<Servlet></Servlet>
 之间添加如下代码
 <loadon-startup>1</loadom-startup>
 2 Servlet容器启动后，客户首次向Servlet发送请求
 3 Servlet类文件被更厚，重新装载Servlet
 Servlet被装在后，容器创建一个Servlet实例并且调用Servlet的init（）方法进行初始化。在Servlet的整个
 生命周期内，init方法只被调用一次
 ```
`装载过程`
```text
1 Servlet容器读取Servlet.Class文件中的数据到内存中
2 Servlet容器创建servletConfig对象。servletConfig对象包含了Servlet的初始化配置信息。
 此外Servlet容器会关联ServletConfig对象和web应用的servletContext对象。
3 Servlet容器创建Servlet对象。
4 Servlet容器调用Servlet对象的init（servletConfig config）方法
通过初始化步骤，创建了Servlet对象的servletConfig对象，并将Servlet对象与的servletConfig对象连。
而config与当前对象的context对象关联。当容器完成Servlet之后，Servlet对象只要通过getServletContext（）方法
就能得到ServletContext对象。
```

service（）方法

```text
service()方法是执行实际任务的主要方法。Servlet容器（Tomcat等）调用service方法来处理来自客户端的请求。并把
相应的结果返回给客户端。
每次Servlet容器接收客户端的请求的时候，容器会产生一个新的线程并调用Servlet实例的Service方法。service方法会检查HTTP的请求类型 
GET POST OUT DELET etc,并在合适的时候调用doGET doPost doPutdDelete方法。所以在编码请求处理逻辑的时候，我们只要关注
doGET doPost的具体实现即可。
```

destroy（）方法

```text
destroy方法也只被`调用一次`，在Servlet的生命周期结束的调用的时候，destroy方法主要用来执行关闭数据库连接 释放资源等。
```
