# tcp网络通信

函数和类的声明文件是**cplus_lib/include/_tcpsocket.h**。

函数和类的定义文件是**cplus_lib/src/_tcpsocket.cpp**。

---

## 概述

对socket通信封装如下：

CTcpClient类：socket通信的客户端类。

CTcpServer类：socket通信的服务端类。

TcpRead函数：接收socket的对端发送过来的数据。

TcpWrite函数：向socket的对端发送数据。

Readn函数：从已经准备好的socket中读取数据。

Writen函数：向已经准备好的socket中写入数据。

## 通信的报文格式

该封装的socket通信报文格式如下：

报文长度+报文内容

报文长度为4字节的整数，表示的是报文内容的长度，而不是整个TCP报文的长度，整个TCP报文的长度是报文内容的长度+4。

报文长度是4字节的整数，即int，是以二进制流的方式写入socket，不是ascii码。

采用CTcpClient类、CTcpServer类、TcpRead函数和TcpWrite函数进行socket通信，可以避免TCP报文粘包的问题。

## socket通信客户端 <!-- {docsify-ignore} -->

### CTcpClient类

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief socket通信的客户端类
 * 
 */
class CTcpClient
{
public:
    int m_sockfd;       //客户端的socket
    char    m_ip[21];       //服务端的ip地址
    int m_port;     //与服务端通信的端口
    bool m_btimeout;    //调用Read和Write方法时，失败的原因是否是超时：true-未超时，false-已超时
    int m_buflen;       //调用Read方法后，接收到报文的大小，单位：字节。

    CTcpClient();           //构造函数

    /**
     * @brief 向服务端发起连接请求
     * 
     * @param ip 服务端的ip地址
     * @param port 服务端监听的端口
     */
    bool ConnectToServer(const char *ip, const int port);

    /**
     * @brief 接收服务端发送过来的数据
     * 
     * @param buffer 接收数据缓冲区的地址，数据的长度存放在m_buflen成员变量中。
     * @param itimeout 等待数据的超时时间，单位：秒，默认为0--无限等待。
     * @return true 成功
     * @return false 失败。主要原因：1）等待超时，成员变量m_btimeout的值被设置为true。2）socket连接已不可用。
     */
    bool Read(char *buffer, const int itimeout = 0);

    /**
     * @brief 向服务端发送数据
     * 
     * @param buffer 待发送数据缓冲区的地址
     * @param ibuflen 待发送数据的大小，单位：字节，默认为0。如果发送的是ascii字符串，ibuflen取0；如果是二进制流数据，ibuflen为二进制数据块的大小。
     * @return true 成功
     * @return false 失败。表示socket连接已不可用
     */
    bool Write(const char *buffer, const int ibuflen = 0);

    /**
     * @brief 断开与服务端的连接
     * 
     */
    void Close();

    ~CTcpClient();      //析构函数自动关闭socket，释放资源。
};
```

<!-- tabs:end -->

## socket通信的服务端 <!-- {docsify-ignore} -->

### CTcpServer类

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief socket通信的服务端类
 * 
 */
class CTcpServer
{
private:
    int m_socklen;     //结构体struct sockaddr_in的大小
    struct sockaddr_in m_clientaddr;        //客户端的地址信息
    struct sockaddr_in m_servaddr;        //服务端的地址信息

public:
    int     m_listenfd;         //服务端用于监听的socket
    int     m_connfd;           //客户端连接上的socket
    bool    m_btimeout;     //调用Read和Write方法时，失败的原因是否是超时：true-未超时，false-已超时。
    int     m_buflen;               //调用Read方法后，接收到的报文的大小，单位：字节。

    CTcpServer();               //构造函数。

    /**
     * @brief 服务端初始化
     * 
     * @param port 指定服务端用于监听的端口。
     * @return true 成功
     * @return false 失败。一般情况下，只要port设置正确，没有被占用，初始化都会成功。
     */
    bool InitServer(const unsigned int port);

    /**
     * @brief 阻塞等待客户端的连接请求
     * 
     * @return true 有新的客户端已连接上来
     * @return false Accept被中断，如果Accept失败，可以重新Accept。
     */
    bool Accept();

    /**
     * @brief 获取客户端的ip地址
     * 
     * @return char* 客户端的IP地址
     */
    char *GetIP();

    /**
     * @brief 接收客户端发送过来的数据
     * 
     * @param buffer 接收数据缓冲区的地址，数据的长度存放在m_buflen成员变量中。
     * @param itimeout 等待数据的超时事件，单位：秒，默认值为0--无限等待。
     * @return true 成功
     * @return false 失败。主要原因：1）等待超时，成员变量m_btimeout的值被设置为true。2）socket连接已不可用。
     */
    bool Read(char *buffer, const int itimeout);

    /**
     * @brief 向客户端发送数据
     * 
     * @param buffer 待发送数据缓冲区的地址
     * @param ibuflen 待发送数据的大小，单位：字节，默认为0。如果发送的是ascii字符串，ibuflen取0；如果是二进制流数据，ibuflen为二进制数据块的大小。
     * @return true 成功
     * @return false 失败。表示socket连接已不可用
     */
    bool Write(const char *buffer, const int ibuflen = 0);

    /**
     * @brief 关闭监听的socket，即m_listenfd，常用于多进程服务程序的子进程代码中。
     * 
     */
    void CloseListen();

    /**
     * @brief 关闭客户端的socket，即m_connfd，常用于多进程服务程序的父进程代码中。
     * 
     */
    void CloseClient(); 

    /**
     * @brief 析构函数自动关闭socket，释放资源。
     * 
     */
    ~CTcpServer();
};
```

<!-- tabs:end -->

## socket通信的函数 <!-- {docsify-ignore} -->

### TcpRead

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 接收socket的对端发送过来的数据
 * 
 * @param sockfd 可用的socket连接
 * @param buffer 接收数据缓冲区的地址
 * @param ibuflen 本次成功接收数据的字节数
 * @param itimeout 接收等待超时的时间，单位：秒，默认为0--无限等待。
 * @return true 成功
 * @return false 失败。主要原因：1）等待超时，成员变量m_btimeout的值被设置为true。2）socket连接已不可用。
 */
bool TcpRead(const int sockfd, char *buffer, int *ibuflen, const int itimeout = 0);
```

<!-- tabs:end -->

### TcpWrite

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 向socket的对端发送数据
 * 
 * @param sockfd 可用的socket连接
 * @param buffer 待发送数据缓冲区的地址
 * @param ibuflen 待发送数据的字节数，如果发送的是ascii字符串，ibuflen取0；如果是二进制流数据，ibuflen为二进制数据块的大小。
 * @return true 成功
 * @return false 失败。表示socket连接已不可用
 */
bool TcpWrite(const int sockfd, const char *buffer, const int ibuflen = 0);
```

<!-- tabs:end -->

### Readn

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 从已准备好的socket中读取数据
 * 
 * @param sockfd 已经准备好的socket连接
 * @param buffer 接收数据缓冲区的地址
 * @param n 本次接收数据的字节数
 * @return true 成功接收到n字节的数据后返回true
 * @return false socket连接已不可用
 */
bool Readn(const int sockfd, char *buffer, const size_t n);
```

<!-- tabs:end -->

### Writen

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 向已准备好的socket中写入数据
 * 
 * @param sockfd 已经准备好的socket连接
 * @param buffer 待发送数据缓冲区的地址
 * @param n 待发送数据的字节数
 * @return true 成功发送n字节的数据后返回true
 * @return false socket连接已不可用
 */
bool Writen(const int sockfd, const char *buffer, const size_t n);
```

<!-- tabs:end -->


