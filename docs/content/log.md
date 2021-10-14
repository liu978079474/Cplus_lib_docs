# 日志文件操作

函数和类的声明文件是**cplus_lib/include/_logfile.h**。

函数和类的定义文件是**cplus_lib/src/_logfile.cpp**。

---

## CLogFile类 <!-- {docsify-ignore} -->

### CLogFile类

CLogFile类用于服务程序记录程序的运行日志。

<!-- tabs:start -->

#### **C++**

```cpp
class CLogFile
{
public:
    FILE    *m_tracefp;     //日志文件指针
    char    m_filename[301];      //日志文件名，建议采用绝对路径
    char    m_openmode[11];     //日志文件的打开方式，一般采用"a+"。
    bool    m_bEnBuffer;                //写入日志时，是否启用操作系统的缓冲机制，默认为不启用
    long    m_MaxLogSize;             //最大日志文件的大小，单位M，默认是100M
    bool    m_bBackup;                  //是否自动切换，日志文件大小超过m_MaxLogSize将自动切换，默认为启用

    /**
     * @brief 构造函数
     * 
     * @param MaxLogSize 最大日志文件的大小，单位M，默认为100M，最小为10M
     */
    CLogFile(const long MaxLogSize = 100);

    /**
     * @brief 打开日志文件
     * 
     * @param filename 日志文件名，建议采用绝对路径，如果文件名中的目录不存在，就先创建目录。
     * @param openmode 日志文件的打开方式，与fopen库函数打开文件的方式相同，默认为"a+"。
     * @param bBackup 是否自动切换，true-切换，false-不切换，在多进程的服务程序中，如果多个进程共用一个日志文件，bBackup必须为false。
     * @param bEnBuffer 是否启用文件缓冲机制，true-启用，false-不启用，如果启用缓冲区，那么写进日志文件中的内容不会立即写入文件，默认为不启用
     */
    bool Open(const char *filename, const char *openmode = 0, bool bBackup = true, bool bEnBuffer = false);

    /**
     * @brief 如果日志文件大于m_MaxLogSize的值，就把当前的日志文件名改为历史日志文件名，再创建新的当前日志文件。
     *                  备份后的文件会在日志文件名后加上日期时间，如：/tmp/log/filetodb.log.2020010123025.
     *  注意：在多进程的程序中，日志文件不可切换，多线程的程序中，日志文件可以切换。
     */
    bool BackupLogFile();

    /**
     * @brief 把内容写入日志文件
     * 
     * @param fmt 可变参数，使用方法与printf库函数相同。
     */
    bool Write(const char *fmt, ...);
    bool WriteEx(const char *fmt, ...);

    /**
     * @brief 关闭日志文件
     * 
     */
    void Close();

    /**
     * @brief 析构函数会调用Close方法。
     * 
     */
    ~CLogFile();
};
```



