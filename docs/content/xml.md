# xml解析

函数和类的声明文件是**cplus_lib/include/_xml.h**。

函数和类的定义文件是**cplus_lib/src/_xml.cpp**。

---

## xml格式字符串的介绍 <!-- {docsify-ignore} -->

xml格式字符串是应用开发中被广泛采用的一种数据格式，简单易懂，容错性和可扩展性非常好，是数据处理、数据通讯和数据交换等应用场景的首选数据格式。

```xml
<data>
<filename>_xml.h</filename><mtime>2020-01-01 12:20:35</mtime><size>1834</size><endl/>
<filename>_xml.cpp</filename><mtime>2020-01-01 10:10:15</mtime><size>5094</size><endl/>
</data>
```

数据集说明：

\<data>：数据集的开始。

\</data>：数据集的结束。

\<endl/>：每行数据的结束。

filename标签：文件名。

mtime标签：文件最后一次被修改的时间。

size标签：文件的大小。

## xml格式字符串的解析 <!-- {docsify-ignore} -->

### GetXMLBuffer

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 解析xml格式字符串的函数族
 *  xml格式的字符串的内容如下
 * <filename>/tmp/freecplus.h</filename><mtime>2020-01-01 12:20:35</mtime><size>18348</size>
 * @param xmlbuffer 待解析的xml格式字符串
 * @param filename 字段的标签名
 * @param value 传入变量的地址，用于存放字段内容，支持bool、int、unsigned int、long、unsigned long、double和char[]
 * 注意：当value参数的数据类型为char[]时，必须保证value参数的内存足够，否则可能发生内存溢出的问题，可以用ilen参数限定获取字段内容的长度，ilen的默认值为0，表示不限长度
 * @return true 成功
 * @return false 如果fieldname参数指定的标签名不存在，则返回失败
 */
bool GetXMLBuffer(const char *xmlbuffer, const char *fieldname, bool *value);
bool GetXMLBuffer(const char *xmlbuffer, const char *fieldname, int *value);
bool GetXMLBuffer(const char *xmlbuffer, const char *fieldname, unsigned int *value);
bool GetXMLBuffer(const char *xmlbuffer, const char *fieldname, long *value);
bool GetXMLBuffer(const char *xmlbuffer, const char *fieldname, unsigned long *value);
bool GetXMLBuffer(const char *xmlbuffer, const char *fieldname, double *value);
bool GetXMLBuffer(const char *xmlbuffer, const char *fieldname, char *value, const int ilen = 0);
```

<!-- tabs:end -->

