# C++ std::string 类详解

## 1. 构造函数 - 初始化

```cpp
// 默认构造函数 - 创建空字符串
string s1;

// 填充构造函数 - n 个相同字符
string s2(10, 'a');  // 结果: "aaaaaaaaaa"

// 从 C 风格字符串构造
const char* str = "hello world";
string s3(str);  // string(const char* s)

// 拷贝构造函数 - 用 string 对象初始化
string s4(s3);  // string(const string& str)

// 指定范围构造
string s5(str, 0, 5);  // 从 str 的第 0 位置开始，取 5 个字符: "hello"

// 字符数组构造
char arr[] = {'H', 'e', 'l', 'l', 'o'};
string s6(arr, 5);  // "Hello"
```

## 2. 赋值操作

```cpp
string str1;
str1 = "hello world";  // 直接赋值 C 字符串

string str2;
str2 = str1;  // 拷贝赋值

string str3;
str3 = 'a';  // 赋值单个字符

// assign() 方法
string str4;
str4.assign(str3);           // 用 string 赋值
str4.assign("hello");        // 用 C 字符串赋值
str4.assign("hellow", 4);   // 用前 n 个字符: "hell"
str4.assign(5, 'a');        // 用 n 个相同字符: "aaaaa"
str4.assign(str1, 0, 5);    // 截取 str1 从位置 0 开始的 5 个字符
```

## 3. 字符串拼接

```cpp
string s1 = "hello";
s1 += " world";  // 拼接 C 字符串

string s2 = "wow";
s1 += s2;  // 拼接 string 对象

s1 += '!';  // 拼接单个字符

// append() 方法
s2.append("hhh");           // 拼接 C 字符串
s2.append("game abc", 4);   // 拼接前 n 个字符: "game"
s2.append(s1, 0, 5);        // 截取 s1 从位置 0 开始的 5 个字符拼接到 s2

// push_back() - 添加单个字符
s2.push_back('z');

// 拼接返回新字符串（不修改原字符串）
string s3 = s1 + " " + s2;
```

## 4. 字符串查找

```cpp
string s = "abcdefgde";

// find() - 从左往右查找
int pos = s.find("de");  // 返回匹配第一个字符的下标，结果: 3
                          // 未找到返回 string::npos (通常是 -1)

// rfind() - 从右往左查找
pos = s.rfind("de");  // 返回匹配最后一个字符的下标，结果: 7

// find_first_of() - 查找任意一个字符首次出现的位置
pos = s.find_first_of("ae");  // 查找 'a' 或 'e'，结果: 0

// find_last_of() - 查找任意一个字符最后出现的位置
pos = s.find_last_of("ae");  // 结果: 8

// find_first_not_of() - 查找第一个不在指定字符集中的位置
pos = s.find_first_not_of("abc");  // 结果: 3 ('d')

// find_last_not_of() - 查找最后一个不在指定字符集中的位置
pos = s.find_last_not_of("defg");  // 结果: 0 ('a')

// 指定查找起始位置
pos = s.find("de", 5);  // 从位置 5 开始查找，结果: 7
```

## 5. 字符串访问

```cpp
string s = "hello world";
char c1 = s[0];   // 使用下标访问，结果: 'h'
char c2 = s.at(6); // 使用 at() 访问（有边界检查），结果: 'w'

char c3 = s.front();  // 获取第一个字符
char c4 = s.back();   // 获取最后一个字符
```

## 6. 字符串修改

```cpp
string s = "hello world";

// 插入
s.insert(5, " XXX");  // 在位置 5 插入: "hello XXX world"
s.insert(0, 3, '!');  // 在位置 0 插入 3 个 '!'

// 删除
s.erase(5, 4);  // 从位置 5 删除 4 个字符
s.pop_back();   // 删除最后一个字符
s.clear();      // 清空字符串

// 替换
s.replace(0, 5, "Hi");  // 将位置 0-5 的替换为 "Hi"

// 交换
string s1 = "hello", s2 = "world";
s1.swap(s2);  // 交换两个字符串
```

## 7. 字符串比较

```cpp
string s1 = "hello", s2 = "world";

if (s1 == s2) { }     // 相等
if (s1 != s2) { }     // 不等
if (s1 < s2) { }      // 字典序比较
if (s1.compare(s2) < 0) { }  // compare() 返回负数/0/正数

// 指定范围比较
s1.compare(0, 3, s2, 0, 3);  // 比较 s1[0:3] 和 s2[0:3]
```

## 8. 字符串大小与容量

```cpp
string s = "hello";

s.size();      // 获取字符串长度（字符数）
s.length();       // 同 size()
s.empty();     // 判断是否为空
s.capacity();  // 获取当前容量
s.max_size();  // 获取最大可能容量
s.resize(10);  // 调整大小，多余填充 '\0'
s.reserve(100); // 预分配容量
```

## 9. 子字符串与迭代

```cpp
string s = "hello world";

string sub = s.substr(0, 5);  // 获取子串，结果: "hello"

// C 风格接口
const char* cstr = s.c_str();  // 返回 C 风格字符串（包含 '\0'）
const char* data = s.data();    // 返回字符数组

// 迭代器遍历
for (auto it = s.begin(); it != s.end(); ++it) {
    cout << *it;
}

// 反向遍历
for (auto it = s.rbegin(); it != s.rend(); ++it) {
    cout << *it;
}
```

## 10. 类型转换

```cpp
// 数字转字符串
string s1 = to_string(123);
string s2 = to_string(3.14);

// 字符串转数字
int n1 = stoi("123");
long n2 = stol("123456");
float n3 = stof("3.14");
double n4 = stod("3.14159");
```

## 11. 常见注意事项

| 注意事项 | 说明 |
|---------|------|
| `find()` 未找到时返回 `string::npos` | 判断查找结果时应与 `string::npos` 比较 |
| `at()` 有边界检查 | 使用 `at()` 比 `[]` 更安全，但稍慢 |
| UTF-8 处理 | `std::string` 按字节处理，中文等多字节字符需特殊处理 |
| 性能考虑 | 频繁拼接时考虑使用 `reserve()` 预分配内存 |
| C 字符串转换 | `c_str()` 返回的指针在 string 修改后可能失效 |
