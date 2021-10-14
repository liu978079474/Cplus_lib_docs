# ftp客户端

函数和类的声明文件是**cplus_lib/include/_ftp.h**。

函数和类的定义文件是**cplus_lib/src/_ftp.cpp**。

---

## Cftp类 <!-- {docsify-ignore} -->

### Cftp类

**Cftp类是在ftplib开源库的基础之上做了二次封装，ftplib开源库的头文件是include/ftplib.h，函数定义文件是src/ftplib.c。**

**ftplib是C代码，用gcc先编译成lib库文件后，再用g++和_ftp.cpp以及目标文件一起编译。**

<!-- tabs:start -->

#### **C++**

```cpp
class Cftp
{
public:
    netbuf *m_ftpconn;      //ftp连接句柄
    unsigned int m_size;        //文件的大小，单位：字节
    char m_mtime[21];       //文件的修改时间，格式：yyyymmddhh24miss

    //以下三个成员变量用于存放login方法登录失败的原因
    bool m_connectfailed;       //连接失败
    bool m_loginfailed;         //登录失败，用户名和密码不正确，或没有登录权限
    bool m_optionfailed;        //设置传输模式失败

    Cftp();
    ~Cftp();

    /**
     * @brief 初始化m_size和m_mtime成员变量
     * 
     */
    void initdata();

    /**
     * @brief 登录ftp服务器
     * 
     * @param host ftp服务器ip地址和端口，中间用":"分隔，如："192.168.1.1:21"
     * @param username 登录ftp服务器用户名
     * @param password 登录ftp服务器的密码
     * @param imode 传输模式，FTPLIB_PASSIVE是被动传输模式，FTPLIB_PORT是主动模式，默认为被动模式
     */
    bool login(const char *host, const char*username, const char *password, const int imode = FTPLIB_PASSIVE);

    /**
     * @brief 注销
     * 
     */
    bool logout();

    /**
     * @brief 获取ftp服务器上文件的时间
     * 
     * @param remotefilename 待获取的文件名
     * @return true 成功，获取到的文件时间存放在m_mtime成员变量中
     * @return false 失败
     */
    bool mtime(const char *remotefilename);

    /**
     * @brief 获取ftp服务器文件的大小
     * 
     * @param remotefilename 待获取的文件名
     * @return true 成功，获取到的文件大小存放在m_size成员变量中
     * @return false 失败
     */
    bool size(const char *remotefilename);

    /**
     * @brief 改变ftp服务器的当前工作目录
     * 
     * @param remotedir ftp服务器上的目录名
     */
    bool chdir(const char *remotedir);

    /**
     * @brief 在ftp服务器上创建目录
     * 
     * @param remotedir ftp服务器上待创建的目录名
     */
    bool mkdir(const char *remotedir);

    /**
     * @brief 删除ftp服务器上的目录
     * 
     * @param remotedir ftp服务器上待删除的目录名
     * @return true 成功
     * @return false 如果权限不足、目录不存在或目录不为空，会返回false
     */
    bool rmdir(const char *remotedir);

    /**
     * @brief 发送NLST命令列出ftp服务器的目录和文件名
     * 
     * @param remotedir ftp服务器的目录名
     * @param listfilename 用于保存从服务器返回的目录和文件名列表
     * 注意：如果列出的是ftp服务器当前目录，remotedir用""，"*"，"."都可以，但是，不规范的ftp服务器可能有差别。
     */
    bool nlist(const char *remotedir, const char *listfilename);

    /**
     * @brief 从ftp服务器上获取文件
     * 
     * @param remotefilename 待获取ftp服务器上的文件名
     * @param localfilename 保存到本地的文件名
     * @param bCheakMTime 文件传输完成后，是否核对远程文件传输前后的时间，保证文件的完整性。
     * 注意：文件在传输的过程中，采用临时文件命名的方法，即在localfilename后加".tmp"，在传输完成后才正式改为localfilename。
     */
    bool get(const char *remotefilename, const char *localfilename, const bool bCheakMTime = true);

    /**
     * @brief 向ftp服务器发送文件
     * 
     * @param localfilename 本地待发送的文件名
     * @param remotefilename 发送到ftp服务器上的文件名
     * @param bCheckSize 文件传输完成后，是否核对远程文件传输前后的时间，保证文件的完整性。
     * 注意：文件在传输的过程中，采用临时文件命名的方法，即在localfilename后加".tmp"，在传输完成后才正式改为localfilename。
     */
    bool put(const char *localfilename, const char *remotefilename, const bool bCheckSize = true);

    /**
     * @brief 删除ftp服务器上的文件
     * 
     * @param remotefilename 待删除的ftp服务器上的文件名
     */
    bool ftpdelete(const char *remotefilename);

    /**
     * @brief 重命名ftp服务器上的文件
     * 
     * @param srcremotefilename ftp服务器上的原文件名
     * @param dstremotefilename ftp服务器上的目标文件名
     */
    bool ftprename(const char*srcremotefilename, const char *dstremotefilename);

    /**
     * @brief 发送LIST命名李处ftp服务器目录中的文件
     * 
     * @param remotedir ftp服务器的目录名
     * @param listfilename 用于保存从服务器返回的目录和文件名列表
     * 注意：如果列出的是ftp服务器当前目录，remotedir用""，"*"，"."都可以，但是，不规范的ftp服务器可能有差别。
     */
    bool dir(const char *remotedir, const char *listfilename);

    /**
     * @brief 向ftp服务器发送site命名
     * 
     * @param command 命令的内容
     */
    bool site(const char *command);

    /**
     * @brief 获取服务器返回信息的最后一条
     * 
     */
    char *response();
};
```

<!-- tabs:end -->