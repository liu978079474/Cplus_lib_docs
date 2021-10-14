# 参数文件操作

函数和类的声明文件是**cplus_lib/include/_inifile.h**。

函数和类的定义文件是**cplus_lib/src/_inifile.cpp**。

---

## CIniFile类 <!-- {docsify-ignore} -->

### CIniFile类

CIniFile类用于服务程序从参数文件中加载参数。

<!-- tabs:start -->

#### **C++**

```cpp
class CIniFile
{
public:
    string  m_xmlbuffer; //存放参数文件全部的内容，由LoadFile方法载入

    CIniFile();

    //把参数文件的内容载入到m_xmlbuffer成员变量中。
    bool LoadFile(const char* filename);

    /**
     * @brief 获取参数的值
     * 
     * @param fieldname 字段的标签名。
     * @param value 传入变量的地址，用于存放字段的值
     * 注意：当value参数的数据类型为char[]时，必须保证value的内存足够，否则可能发生内存溢出的问题，
     *              也可以用ilen参数限定获取字段内容的长度，ilen的默认值为0，表示不限定获取字段内容的长度。
     */
    bool GetValue(const char *fieldname, bool *value);
    bool GetValue(const char *fieldname, int *value);
    bool GetValue(const char *fieldname, unsigned int *value);
    bool GetValue(const char *fieldname, long *value);
    bool GetValue(const char *fieldname, unsigned long *value);
    bool GetValue(const char *fieldname, double *value);
    bool GetValue(const char *fieldname, char *value, const int ilen = 0);
};
```

<!-- tabs:end -->


