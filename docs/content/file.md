# 文件操作

函数和类的声明文件是**cplus_lib/include/_file.h**。

函数和类的定义文件是**cplus_lib/src/_file.cpp**。

---

## 删除文件 <!-- {docsify-ignore} -->

### REMOVE

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 删除文件，类似Linux系统的rm命令
 * 
 * @param filename 待删除的文件名，建议采用绝对路径的文件名
 * @param times 执行删除文件的次数，默认为1。
 * @return true 成功
 * @return false 失败。主要原因是权限不足。
 */
bool REMOVE(const char *filename, const int times = 1);
```

<!-- tabs:end -->

## 文件重命名 <!-- {docsify-ignore} -->

### RENAME

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 重命名文件，类似Linux系统的mv命令
 * 
 * @param srcfilename 原文件名，建议采用绝对路径的文件名。
 * @param dstfilename 目标文件名，建议采用绝对路径的文件名。
 * @param times 执行重命名文件的次数，默认为1。
 * @return true 成功
 * @return false 失败。主要原因是权限不足或磁盘空间不够，如果原文件和目标文件不在同一个磁盘分区，重命名也可能失败
 */
bool RENAME(const char *srcfilename, const char *dstfilename, const int times = 1);
```

<!-- tabs:end -->

## 复制文件 <!-- {docsify-ignore} -->

### COPY

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 复制文件，类似Linux系统的cp命令
 * 
 * @param srcfilename 原文件名，建议采用绝对路径的文件名。
 * @param dstfilename 目标文件名，建议采用绝对路径的文件名。
 * @return true 成功
 * @return false 失败。主要原因是权限不足或磁盘空间不够。
 * 注意：
 * 1）在复制文件之前，会自动创建destfilename参数中的目录名。
 * 2）复制文件的过程中，采用临时文件命名的方法，复制完成后再改名为destfilename，避免中间状态的文件被读取。
 * 3）复制后的文件的时间与原文件相同，这一点与Linux系统cp命令不同。
 */
bool COPY(const char *srcfilename, const char *dstfilename);
```

<!-- tabs:end -->

## 获取文件的大小 <!-- {docsify-ignore} -->

### FileSize

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 获取文件的大小
 * 
 * @param filename 待获取的文件名，建议采用绝对路径的文件名。
 * @return int 如果文件不存在或没有访问权限，返回-1，成功放回文件的大小，单位是字节。
 */
int FileSize(const char *filename);
```

<!-- tabs:end -->

## 获取文件的时间 <!-- {docsify-ignore} -->

### FileMTime

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 获取文件的时间
 * 
 * @param filename 待获取的文件名，建议采用绝对路径的文件名。
 * @param mtime 用于存放文件的时间，即stat结构体的st_mtime。
 * @param fmt 设置时间的输出格式，默认为"yyyymmddhh24miss"。
 * @return true 成功
 * @return false 失败。主要原因：文件不存在或没有访问权限
 */
bool FileMTime(const char *filename, char *mtime, const char *fmt = 0);
```

<!-- tabs:end -->

## 重置文件的时间 <!-- {docsify-ignore} -->

### UTime

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 重置文件的修改时间属性
 * 
 * @param filename 待重置的文件名，建议采用绝对路径的文件名。
 * @param mtime 字符串表示的时间，格式不限，但一定要包括yyyymmddhh24miss
 * @return true 成功
 * @return false 失败，原因保存在errno中
 */
bool UTime(const char *filename, const char *mtime);
```

<!-- tabs:end -->

## 打开文件 <!-- {docsify-ignore} -->

### *FOPEN

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 打开文件
 * 
 * @param filename 调用fopen库函数打开文件，如果文件名中包含的目录不存在，就创建目录。
 * @param mode 与fopen函数的参数完全相同
 * @return FILE* 与fopen函数的参数完全相同
 */
FILE *FOPEN(const char *filename, const char *mode);
```

<!-- tabs:end -->

## 读取文件 <!-- {docsify-ignore} -->

### FGETS

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 从文本文件中读取一行
 * 
 * @param fp 已打开的文件指针
 * @param buffer 用于存放读取的内容，buffer必须大于readsize + 1，否则可能会造成读到的数据不完整或者内存的溢出。
 * @param readsize 本次打算读取的字节数，如果已经读取到了行结束标志，函数返回
 * @param endbz 行内容结束的标志，默认为空，表示行内容以"\n"为结束标志。
 * @return true 成功
 * @return false 失败，一般情况下，失败可以认为是文件已结束。
 */
bool FGETS(const FILE *fp, char *buffer, const int readsize, const char *endbz = 0);
```

<!-- tabs:end -->

## 文件操作 <!-- {docsify-ignore} -->

### CFile类

CFile对常用的文件操作功能做了封装。

<!-- tabs:start -->

#### **C++**

```cpp
class CFile
{
private:
    FILE *m_fp;     //文件指针
    bool m_bEnBuffer;        //是否启用缓冲，true-启用；false-不启用，默认是启用。
    char m_filename[301];       //文件名，建议采用绝对路径的文件名。
    char m_filenametmp[301];        //临时文件名，在m_filename后加".tmp"。

public:
    CFile();        //构造函数

    bool IsOpened();        //判断文件是否已打开，返回值：true-已打开；false-未打开。

    /**
     * @brief 打开文件
     * 
     * @param filename 待打开的文件名，建议采用绝对路径的文件名。
     * @param openmode 打开文件的模式，与fopen库函数的打开模式相同。
     * @param bEnBuffer 是否启用缓冲，true-启用；false-不启用，默认是启用。
     * 注意：如果待打开的文件目录不存在，就会创建目录。
     */
    bool Open(const char *filename, const char *openmode, bool bEnBuffer = true);

    /**
     * @brief 关闭文件指针，并删除文件
     * 
     */
    bool CloseAndRemove();

    /**
     * @brief 专为重命名而打开文件，参数与Open方法相同
     * 注意：OpenForRename打开的是filename后加".tmp"的临时文件，所以openmode只能是"a"、 "a+"、 "w"、 "w+"。
     */
    bool OpenForRename(const char *filename, const char *openmode, bool bEnBuffer = true);

    /**
     * @brief 关闭文件指针，并把OpenForRename方法打开的临时文件名重命名为filename。
     * 
     */
    bool CloseAndRename();

    /**
     * @brief 调用fprintf向文件写入数据，参数与fprintf库函数相同，但不需要传入文件指针。
     * 
     */
    void Fprintf(const char *fmt,...);

    /**
     * @brief 从文件中读取以换行符"\n"结束的一行。
     * 
     * @param buffer 用于存放读取的内容，buffer必须大于readsize + 1，否则可能会造成内存的溢出。
     * @param readsize 本次打算读取的字节数，如果已经读取到了结束标志"\n"，函数返回。
     * @param bdelcrt 是否删除行结束标志"\r"和"\n"，true-删除；false-不删除，默认为false。
     * @return true 成功。
     * @return false 失败。一般情况下，失败可以认为是文件已结束。
     */
    bool Fgets(char *buffer, const int readsize, bool bdelcrt = false);

    /**
     * @brief 从文件中读取一行
     * 
     * @param buffer 用于存放读取的内容，buffer必须大于readsize + 1，否则可能会造成读到的数据不完整或内存的溢出
     * @param readsize 本次打算读取的字节数，如果已经读取到了结束标志，函数返回。
     * @param endbz 行内容结束的标志，默认为空，表示行内容以"\n"为结束标志。
     * @return true 成功
     * @return false 失败。一般情况下，失败可以认为是文件已结束。
     */
    bool FFGETS(char *buffer, const int readsize, const char *endbz = 0);

    /**
     * @brief 从文件中读取数据块
     * 
     * @param ptr 用于存放读取的内容
     * @param size 本次打算读取的字节数
     * @return size_t 本次从文件中成功读取的字节数，如果文件未结束，返回值等于size，如果文件已结束，返回值为实际读取的字节数。
     */
    size_t Fread(void *ptr, size_t size);

    /**
     * @brief 向文件中写入数据块
     * 
     * @param ptr 待写入数据块的地址。
     * @param size 待写入数据块的字节数。
     * @return size_t 本次成功写入的字节数，如果磁盘空间足够，返回值等于size。
     */
    size_t Fwrite(const void *ptr, size_t size);

    /**
     * @brief 关闭文件指针，如果存在临时文件，就删除它。
     * 
     */
    void Close();

    ~CFile();       //析构函数会调用Close方法。
};
```

<!-- tabs:end -->





