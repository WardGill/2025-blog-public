# C++ string 学习笔记

> `#include <string>` 本质是一个类，很多操作都提供 `const string&` 和 `const char*` 两种形式，分别对应 C++ 的 string 类型和 C 语言的字符串。

---

## 一、构造函数与初始化

| 方式 | 代码示例 | 说明 |
|------|----------|------|
| 默认构造 | `string s1;` | 创建空字符串 |
| 填充构造 | `string s2(10, 'a');` | 创建包含 n 个相同字符的字符串 |
| C 字符串构造 | `string s3("hello world");` | 用 C 字符串初始化 |
| 拷贝构造 | `string s4(s3);` | 用 string 对象初始化 |
| 子串构造 | `string s5(s3, 0, 5);` | 从 s3 的起始位置取 n 个字符 |

```cpp
// 构造函数示例
string s1;                          // ""
string s2(10, 'a');                  // "aaaaaaaaaa"
const char* str = "hello world";
string s3(str);                      // "hello world"
string s4(s3);                       // "hello world"
string s5(s3, 0, 5);                 // "hello" (从位置0开始取5个字符)
```

---

## 二、赋值操作

| 方法 | 代码示例 |
|------|----------|
| `=` 赋值 | `str1 = "hello";` 或 `str2 = str1;` 或 `str3 = 'a';` |
| `assign()` | `str.assign(str2);` 或 `str.assign("hello");` |

```cpp
string str1;
str1 = "hello world";               // 赋值字符串字面量

string str2;
str2 = str1;                        // 赋值另一个 string 对象

string str3;
str3 = 'a';                         // 赋值单个字符

string str4;
str4.assign(str3);                   // "a"
str4.assign("hello");               // "hello"
str4.assign("hellow", 4);           // "hell" (前n个字符)
str4.assign(5, 'a');                // "aaaaa" (n个字符)
```

---

## 三、字符串拼接

| 方法 | 代码示例 | 说明 |
|------|----------|------|
| `+=` 运算符 | `s1 += " world";` | 拼接字符串或字符 |
| `append()` | `s2.append("hhh");` | 拼接字符串 |

```cpp
// += 运算符拼接
string s1 = "hello";
s1 += " world";                     // "hello world"
string s2 = "wow";
s1 += s2;                           // "hello worldwow"

// append() 方法拼接
string s3 = "abc";
s3.append("hhh");                   // "abchhhh"
s3.append("game abc", 4);           // "abchhhhgame" (拼接前n个字符)
s3.append(s1, 0, 5);                // "abchhhhgamehello" (截取s1从位置0开始的5个字符)
```

---

## 四、字符串查找

| 方法 | 说明 | 返回值 |
|------|------|--------|
| `find()` | 从左往右查找 | 找到返回第一个字符下标，未找到返回 `string::npos` |
| `rfind()` | 从右往左查找 | 找到返回第一个字符下标，未找到返回 `string::npos` |

```cpp
string s = "abcdefgde";
int pos = s.find("de");             // 返回 3 (下标从0开始)
int pos2 = s.find("xyz");           // 返回 string::npos (未找到)

// rfind() 从右往左查找
int pos3 = s.rfind("de");           // 返回 7 ("abcdefgde" 中最后一个 "de" 的位置)

// 检查是否找到
if (pos3 != string::npos) {
    cout << "Found at position: " << pos3 << endl;
}
```

---

## 五、字符串替换查找

```cpp
string str1 = "abcdefg";
str1.replace(1, 3, "1111");         // "a1111efg"
// 说明：从下标1开始，删除3个字符，替换为 "1111"

// 替换出现的所有子串
string text = "hello world hello";
string search = "hello";
string replace = "hi";
size_t pos = 0;
while ((pos = text.find(search, pos)) != string::npos) {
    text.replace(pos, search.length(), replace);
    pos += replace.length();
}
// 结果: "hi world hi"
```

---

## 六、字符串比较

### compare() 方法返回值

| 比较结果 | 返回值 |
|----------|--------|
| 相等 | `0` |
| 大于 | `正数` |
| 小于 | `负数` |

### ASCII 比较规则

1. 逐字符、从左到右比较 ASCII 码值
2. 值大的字符串整体更大
3. 如果前面的字符都相等，长度更长的字符串更大

```cpp
string str1 = "abc";
string str2 = "bcd";

int result = str1.compare(str2);    // 返回负数 ('a' < 'b')

// 也可以直接使用比较运算符
if (str1 == str2) { }               // 相等
if (str1 > str2) { }                // 大于
if (str1 < str2) { }                // 小于

// 部分比较
string s = "abcdef";
s.compare(1, 3, "bcd");             // 比较子串 "bcd" 与 "bcd"，返回 0
```

---

## 七、字符存取

```cpp
string str = "hello";

// 方法1: [] 运算符 (不进行越界检查)
char c1 = str[1];                    // 'e'
str[2] = 'L';                        // 修改字符

// 方法2: at() 方法 (进行越界检查，越界抛出异常)
char c2 = str.at(1);                 // 'e'
try {
    char c3 = str.at(10);            // 越界，抛出 std::out_of_range 异常
} catch (const out_of_range& e) {
    cerr << e.what() << endl;
}

// front() 和 back()
char first = str.front();            // 第一个字符
char last = str.back();              // 最后一个字符
```

---

## 八、字符串大小与容量

| 方法 | 说明 |
|------|------|
| `size()` / `length()` | 返回字符串长度 |
| `empty()` | 判断字符串是否为空 |
| `capacity()` | 返回当前分配的存储空间大小 |
| `reserve(n)` | 预留 n 个字符的空间 |
| `resize(n)` | 调整字符串大小 |
| `clear()` | 清空字符串 |

```cpp
string s = "hello";

cout << s.size() << endl;            // 5
cout << s.length() << endl;         // 5
cout << s.empty() << endl;          // false

// 预留空间
s.reserve(100);                     // 避免多次重新分配

// 调整大小
s.resize(10, 'x');                  // "helloxxxxx" (用字符填充)
s.resize(3);                        // "hel" (截断)

// 清空
s.clear();                          // ""
```

---

## 九、字符串插入与删除

```cpp
string s1 = "hello";

// insert() - 插入
s1.insert(1, "aha");               // "hahaello" (从下标1插入字符串)
s1.insert(1, 4, 'c');              // "hccccahaello" (从下标1插入n个字符)
s1.insert(0, s1);                  // 在开头插入整个字符串

// erase() - 删除
s1.erase(1, 3);                    // "hahaello" (从下标1删除3个字符)
s1.erase(s1.begin());              // 删除第一个字符
s1.erase(s1.begin() + 2, s1.end()); // 删除从第3个字符到末尾

// pop_back() - 删除最后一个字符
s1.pop_back();
```

---

## 十、子串提取

```cpp
string s1 = "hello world";

// substr() - 获取子串
string sub1 = s1.substr(0, 5);      // "hello" (从下标0开始取5个字符)
string sub2 = s1.substr(6);         // "world" (从下标6开始到末尾)
```

---

## 十一、其他常用操作

### 字符与数字转换

```cpp
#include <sstream>

// 字符串转数字
string str = "123";
int num = stoi(str);                // 123
double d = stod(str);               // 123.0

// 数字转字符串
int value = 456;
string s = to_string(value);        // "456"

// 使用 stringstream
stringstream ss;
ss << value;
string result = ss.str();           // "456"
```

### 大小写转换

```cpp
#include <algorithm>

string s = "Hello";

// 转小写
transform(s.begin(), s.end(), s.begin(), ::tolower);
// 结果: "hello"

// 转大写
transform(s.begin(), s.end(), s.begin(), ::toupper);
// 结果: "HELLO"
```

### 去除空白字符

```cpp
string s = "  hello world  ";

// 去除前后空白
s.erase(0, s.find_first_not_of(" \t\n\r\f\v"));  // 去除前导空白
s.erase(s.find_last_not_of(" \t\n\r\f\v") + 1);  // 去除尾部空白
// 结果: "hello world"
```

### 分割字符串

```cpp
#include <sstream>
#include <vector>

string text = "apple,banana,cherry";
stringstream ss(text);
string token;
vector<string> tokens;

while (getline(ss, token, ',')) {
    tokens.push_back(token);
}
// tokens: ["apple", "banana", "cherry"]
```

---

## 十二、实用示例

### 示例1：从邮箱地址获取用户名

```cpp
string email = "zhangsan@163.com";

// 找到 '@' 的位置
size_t pos = email.find('@');
if (pos != string::npos) {
    string username = email.substr(0, pos);
    cout << "用户名: " << username << endl;
}
// 输出: 用户名: zhangsan
```

### 示例2：判断字符串是否为回文

```cpp
bool isPalindrome(const string& s) {
    int left = 0;
    int right = s.length() - 1;
    while (left < right) {
        if (s[left] != s[right]) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}
```

### 示例3：统计字符出现次数

```cpp
int countChar(const string& s, char target) {
    int count = 0;
    for (char c : s) {
        if (c == target) {
            count++;
        }
    }
    return count;
}

// 或者使用 STL
int count2 = count(s.begin(), s.end(), 'a');
```

---

## 十三、注意事项

1. **`find()` 未找到时返回 `string::npos`**，而不是 `-1`（虽然两者值相等，但应使用 `string::npos`）
2. **`[]` 不检查越界**，`at()` 会检查越界并抛出异常
3. **C 风格字符串需要以 `\0` 结尾**，但 `string` 不需要
4. **迭代器失效**：插入、删除等操作可能使迭代器失效
5. **性能考虑**：频繁拼接时，使用 `reserve()` 预分配空间以提高性能

---

## 十四、常用函数速查表

| 分类 | 函数 | 功能 |
|------|------|------|
| 构造 | `string()` | 默认构造 |
| | `string(n, char)` | 填充 n 个字符 |
| | `string(const char*)` | 从 C 字符串构造 |
| 赋值 | `assign()` | 赋值 |
| 拼接 | `append()` | 拼接字符串 |
| | `operator+=` | 拼接运算符 |
| 查找 | `find()` | 查找子串 |
| | `rfind()` | 反向查找 |
| | `find_first_of()` | 查找任意字符 |
| | `find_last_of()` | 反向查找任意字符 |
| 修改 | `insert()` | 插入 |
| | `erase()` | 删除 |
| | `replace()` | 替换 |
| | `clear()` | 清空 |
| 访问 | `operator[]` | 下标访问 |
| | `at()` | 带检查的下标访问 |
| | `front()`, `back()` | 首尾字符 |
| 容量 | `size()`, `length()` | 长度 |
| | `empty()` | 判空 |
| | `capacity()` | 容量 |
| | `reserve()` | 预留空间 |
| | `resize()` | 调整大小 |
| 子串 | `substr()` | 获取子串 |
| 比较 | `compare()` | 比较 |
| | `operator==, <, >` | 比较运算符 |
