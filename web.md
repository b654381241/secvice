Author:水木黄昏

# WEB

## TCP&UDP

1、TCP是面向连接的，两者通信需要建立连接，端对端的传输。UDP是面向无连接的，发送数据之前不需要建立连接。

2、TCP提供可靠的服务，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达；UDP不保证可靠交付。

3、UDP具有较好的实时性，工作效率比TCP高，适用于对高速传输和实时性有较高的通信或广播通信。

4、每一条TCP连接只能是点到点的;UDP支持一对一，一对多，多对一和多对多的交互通信

5、TCP对系统资源要求较多，UDP对系统资源要求较少。

### TCP

![img](https://upload-images.jianshu.io/upload_images/13103677-9fe8a7c2d01f398c.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/625/format/webp)

![img](https://upload-images.jianshu.io/upload_images/13103677-295b536a4c566d81.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/611/format/webp)

![img](https://img2018.cnblogs.com/blog/922925/201906/922925-20190606092100062-132869608.png)

![img](https://img-blog.csdn.net/20130602151600953)

### UDP

UDP编程的服务器端一般步骤是：

1、创建一个socket，用函数socket()；

2、设置socket属性，用函数setsockopt();* 可选

3、绑定IP地址、端口等信息到socket上，用函数bind();

4、循环接收数据，用函数recvfrom();

5、关闭网络连接；

UDP编程的客户端一般步骤是：

1、创建一个socket，用函数socket()；
2、设置socket属性，用函数setsockopt();* 可选
3、绑定IP地址、端口等信息到socket上，用函数bind();* 可选
4、设置对方的IP地址和端口等属性;
5、发送数据，用函数sendto();
6、关闭网络连接；

## HTTP&HTTPs

| 方法    | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| get     | 请求指定的页面信息。（向服务器请求指定的资源）               |
| head    | 类似于get请求，只不过返回的响应中没有具体的内容，用于获取报头 |
| post    | 向指定资源提交数据进行处理请求，例如提交表单或者上传文件。数据被包含在请求体中。post请求可能会导致新的资源的建立或已有资源的修改。 |
| put     | 向服务器上传指定的内容                                       |
| delete  | 请求服务器删除Request-URL所标识的资源。                      |
| connect | HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器。     |
| options | 返回服务器对指定资源数据支持的HTTP请求方法，一般用于测试服务器功能的可用性。 |
| trace   | 回显服务器收到的请求，主要用于测试或诊断                     |

## GET&POST

1.GET提交的数据会放在URL之后，以？分隔URL和传输数据，参数之间以&相连，如[https://www.baidu.com/s?ie=UTF-8&wd=wsgi](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.baidu.com%2Fs%3Fie%3DUTF-8%26wd%3Dwsgi)

2.提交参数的大小不同。GET产生一个TCP数据包；POST产生两个TCP数据包。

3.安全问题

GET方式提交数据，会带来安全问题，因为参数是裸露在地址栏上，所以不安全。

POST方式参数在body中，所以安全性较高。（注意：在http协议下，不管哪种提交方式，都是明码提交，只要有抓包工具，都能抓取数据。）

4.是否浏览器可以收藏

GET请求因为参数在地址栏上，因此可以收藏。而POST请求不行，不能被浏览器收藏，因为参数无法被浏览器保存。