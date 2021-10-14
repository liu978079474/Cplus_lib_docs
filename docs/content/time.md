# 时间和计时器操作

函数和类的声明文件是**cplus_lib/include/_time.h**。

函数和类的定义文件是**cplus_lib/src/_time.cpp**。

---

## 获取操作系统的时间 <!-- {docsify-ignore} -->

### LocalTime

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 获取操作系统的时间
 * 
 * @param stime 用于存放获取到的时间字符串
 * @param timetv1 时间的偏移量，单位：秒，0是默认值，表示当前时间，30表示当前时间30秒之后的时间点，-30表示当前时间30秒之前的时间点。
 * @param fmt 输出时间的格式，fmt每部分的含义：yyyy-年份；mm-月份；dd-日期；hh24-小时；mi-分钟；ss-秒，默认是"yyyy-mm-dd hh24:mi:ss"，
 * 目前支持以下格式：
 * "yyyy-mm-dd hh24:mi:ss"
 * "yyyymmddhh24miss"
 * "yyyy-mm-dd"
 * "yyyymmdd"
 * "hh24:mi:ss"
 * "hh24miss"
 * "hh24:mi"
 * "hh24mi"
 * "hh24"
 * "mi"
 */
void LocalTime(char *stime, const char *fmt = 0, const int timetv1 = 0);
```

<!-- tabs:end -->

## 时间转换函数 <!-- {docsify-ignore} -->

### timetostr

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 把整数表示的时间转换为字符串表示的时间
 * 
 * @param ltime 整数表示的时间
 * @param stime 字符串表示的时间
 * @param fmt 输出字符串时间stime的格式，与LocalTime函数的fmt参数相同
 */
void timetostr(const time_t ltime, char *stime, const char *fmt = 0);
```

<!-- tabs:end -->

### strtotime

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 把字符串表示的时间转换为整数表示的时间
 * 
 * @param stime 字符串表示的时间，格式不限，但要包括yyyymmddhh24miss
 * @return time_t 整数表示的时间，如果stime的格式不正确，则返回-1
 */
time_t strtotime(const char *stime);
```

<!-- tabs:end -->

## 时间的运算 <!-- {docsify-ignore} -->

### AddTime

<!-- tabs:start -->

#### **C++**

```cpp
/**
 * @brief 把字符串表示的时间加上一个偏移的秒数后得到一个新的字符串表示的时间
 * 
 * @param in_stime 输入的字符串格式的时间，格式不限。
 * @param out_stime 输出的字符串格式的时间
 * @param timev1 需要偏移的秒数，正数往后偏移，负数往前偏移
 * @param fmt 输出字符串时间out_stime的格式，与LocalTime函数的fmt参数相同
 * @return true 成功
 * @return false 失败，in_stime的格式不正确
 */
bool AddTime(const char *in_stime, char *out_stime, const int timev1, const char *fmt = 0);
```

<!-- tabs:end -->

## 计时器 <!-- {docsify-ignore} -->

### CTimer类

CTimer类是一个精确到微秒的计时器。

<!-- tabs:start -->

#### **C++**

```cpp
//精确到微妙的计时器
class CTimer
{
private:
    struct timeval m_start;     //开始计时的时间
    struct timeval m_end;       //计时完成的时间

    //开始计时
    void Start();

public:
    CTimer();       //构造函数中会调用Start方法

    //计算已开始计时的时间，单位秒，小数点后面是微妙
    double Elapsed();
};
```

<!-- tabs:end -->


