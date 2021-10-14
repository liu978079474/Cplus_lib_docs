# 字符串操作

函数和类的声明文件是**cplus_lib/include/_string.h**。

函数和类的定义文件是**cplus_lib/src/_string.cpp**。

---

## 字符串复制 <!-- {docsify-ignore} -->

### STRCPY

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 安全的字符串复制函数。对字符串进行复制。
 * 
 * @param dest 目标字符串，不需要初始化，在STRCPY函数中会对它进行初始化。
 * @param destlen 目标字符串dest占用内存的大小。
 * @param src 原字符串。
 * @return char* 目标字符串dest的地址。
 */
char *STRCPY(char* dest, const size_t destlen, const char* src);
```

<!-- tabs:end -->

### STRNCPY

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 安全的strncpy函数
 * 
 * @param dest 目标字符串，不需要初始化，在STRNCPY函数中会对它进行初始化。
 * @param destlen 目标字符串dest占用内存的大小。
 * @param src 原字符串。
 * @param n 待复制的字节数。
 * @return char* 目标字符串dest的地址
 */
char *STRNCPY(char* dest, const size_t destlen, const char* src, size_t n);
```

<!-- tabs:end -->

## 字符串拼接 <!-- {docsify-ignore} -->

### STRCAT

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 安全的strcat函数。在原有的字符串内容上加上待追加的内容。
 * 
 * @param dest 目标字符串
 * @param destlen 目标字符串dest占用内存的大小
 * @param src 待追加的字符串
 * @return char* 目标字符串dest的地址
 */
char *STRCAT(char* dest, const size_t destlen, const char* src);
```

<!-- tabs:end -->

### STRNCAT

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 安全的strncat函数。在原有的内容加上待追加的内容（可以选择待追加的字节数）
 * 
 * @param dest 目标字符串，注意，如果dest从未使用过，那么需要至少一次初始化
 * @param destlen 目标字符串dest占用内存的大小
 * @param src 待追加字符串
 * @param n 待追加的字节数
 * @return char* 目标字符串dest的地址
 */
char *STRNCAT(char* dest, const size_t destlen, const char* src, size_t n);
```

<!-- tabs:end -->

## 格式化输出到字符串 <!-- {docsify-ignore} -->

### SPRINTF

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 安全的sprintf函数。格式化输出到字符串
 * 
 * @param dest 输出字符串，不需要初始化，在SPRINTF函数中会对它进行初始化
 * @param destlen 输出字符串dest占用内存的大小，如果格式化后的字符串内容的长度大于destlen-1，后面的内容将丢弃。
 * @param fmt 格式控制描述
 * @param ... 填充到格式控制描述fmt中的参数
 * @return int 格式化后的内容长度，一般不关心返回值
 */
int SPRINTF(char* dest, const size_t destlen, const char *fmt, ...);
```

<!-- tabs:end -->

### SNPRINTF

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 安全的snprintf函数。格式化输出到字符串。
 * 
 * @param dest 输出字符串，不需要初始化，在SNPRINTF函数中会对它进行初始化
 * @param destlen 输出字符串dest占用内存的大小，如果格式化后的字符串内容的长度大于destlen-1，后面的内容将丢弃。
 * @param n 把格式化后的字符串截取n-1存放到dest中，如果n > destlen，则取destlen - 1。
 * @param fmt 格式控制描述
 * @param ... 填充到格式控制描述fmt中的参数
 * @return int 格式化后的内容长度，一般不关心返回值
 */
int SNPRINTF(char* dest, const size_t destlen, size_t n, const char *fmt, ...);
```

<!-- tabs:end -->

## 删除字符串左、右和两边字符 <!-- {docsify-ignore} -->

### DeleteLChar

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 删除字符串左边指定的字符。
 * 
 * @param str 待处理的字符串
 * @param chr 需要删除的字符
 */
void DeleteLChar(char *str, const char chr);
```

<!-- tabs:end -->

### DeleteRChar

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 删除字符串右边指定的字符
 * 
 * @param str 待处理的字符串
 * @param chr 需要删除的字符
 */
void DeleteRChar(char *str, const char chr);
```

<!-- tabs:end -->

### DeleteLRChar

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 删除字符串左右两边指定的字符
 * 
 * @param str 待处理的字符串
 * @param chr 需要删除的字符
 */
void DeleteLRChar(char *str, const char chr);
```

<!-- tabs:end -->

## 字符串大小写转换 <!-- {docsify-ignore} -->

### ToUpper

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 把字符串中的小写字母转换成大写，忽略不是字母的字符
 * 
 * @param str 待转换的字符串，支持char[]和string两种类型
 */
void ToUpper(char *str);
void ToUpper(string &str);
```

<!-- tabs:end -->

### ToLower

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 把字符串中的大写字母转换成小写，忽略不是字母的字符
 * 
 * @param str 待转换的字符串，支持char[]和string两种类型
 */
void ToLower(char *str);
void ToLower(string &str);
```

<!-- tabs:end -->

## 字符串替换 <!-- {docsify-ignore} -->

### UpdateStr

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 字符串替换函数。在字符串str中，如果存在字符串str1，就替换为字符串str2。
 * 
 * @param str 待处理的字符串
 * @param str1 需要被替换的旧内容
 * @param str2 替换的新内容
 * @param bloop 是否循环执行替换
 * 注意：
 * 1、如果str2比str1要长，替换后str会变长，所以必须保证str有足够的长度，否则内存会溢出。
 * 2、如果str2中包含了str1的内容，且bloop为true，存在逻辑错误，将不执行任何替换
 */
void UpdateStr(char *str, const char *str1, const char *str2, bool bloop = true);
```

<!-- tabs:end -->

## 从字符串中提取数字 <!-- {docsify-ignore} -->

### PickNumber

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 从一个字符串中提取出数字的内容，存放在另一个字符串中
 * 
 * @param src 源字符串
 * @param dest 目标字符串
 * @param bsigned 是否包括符号（+和-），true--包括；false--不包括
 * @param bdot 是否包括小数点的圆点符号，true--包括；false--不包括
 */
void PickNumber(const char* src, char* dest, const bool bsigned, const bool bdot);
```

<!-- tabs:end -->

## 正则表达式 <!-- {docsify-ignore} -->

### MatchStr

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 正则表达式，判断一个字符串是否匹配另一个字符串。
 * 
 * @param str 需要判断的字符串，精确表示的字符串，如文件名“freecplus.cpp”
 * @param rules 匹配规则表达式，用星号“*”表示任意字符串，多个字符串之间用半角的逗号分隔，如“*.h,*.cpp”
 * 注意：str参数不支持“*”，rules参数支持“*”，函数在判断str是否匹配rules的时候，会忽略字母的大小写。
 */
bool MatchStr(const string str, const string rules);
```

<!-- tabs:end -->

## 统计字符串的字数 <!-- {docsify-ignore} -->

### Words

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 统计字符串的字数，全角和半角的汉字和全角的标点符号算一个字。
 * 
 * @param str 待统计的字符串
 * @return int 字符串str的字数
 */
int Words(const char* str);
```

<!-- tabs:end -->

## 字符串的拆分 <!-- {docsify-ignore} -->

CCmdStr类用于拆分用分隔符分隔字段内容的字符串。

字符串的格式为：字段内容1+分隔符+字段内容2+分隔符+字段内容3+分隔符+....+字段内容n。

### CCmdStr类

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief CCmdStr类用于拆分有分隔符的字符串
 * 字符串的格式为：字段内容1 + 分隔符 + 字段内容2 + 分隔符 + 字段内容3 + 分隔符 + ... + 字段内容n
 */
class CCmdStr
{
public:
    vector<string>  m_vCmdStr;      //      存放拆分后的字段内容

    CCmdStr();      //      构造函数

    /**
     * @brief 把字符串拆分到m_vCmdStr容器中
     * 
     * @param buffer 待拆分的字符串
     * @param sepstr buffer中采用的分隔符，注意，sepstr参数的数据类型不是字符，是字符串，如：“，”、“ ”、“|”、“~!~”。
     * @param bdelspace 拆分后是否删除字段内容前后的空格，true--删除；false--不删除，默认为删除
     */
    void SplitToCmd(const string buffer, const char* sepstr, const bool bdelspace=true);

    /**
     * @brief 获取拆分后字段的个数，即m_vCmdStr容器的大小
     * 
     * @return int m_vCmdStr容器的大小
     */
    int CmdCount();

    /**
     * @brief 从m_vCmdStr容器获取字段内容
     * 
     * @param inum 字段的顺序好，类似数组的下标，从0开始
     * @param value 传入变量的地址，用于存放字段内容
     * @param ilen 存放字段内容的长度
     * @return true 成功
     * @return false 如果inum的取值超出了m_vCmdStr容器的大小，返回失败
     */
    bool GetValue(const int inum, char* value, const int ilen = 0);     //      字符串，ilen默认值为0
    bool GetValue(const int inum, int *value);      //      int整数
    bool GetValue(const int inum, unsigned int *value);     //      unsigned int整数
    bool GetValue(const int inum, long *value);     //      long整数
    bool GetValue(const int inum, unsigned long *value);        //      unsigned long整数
    bool GetValue(const int inum, double *value);       //      双精度double
    bool GetValue(const int inum, bool *value);     //      bool型

    ~CCmdStr();     //      析构函数
};
```

<!-- tabs:end -->
