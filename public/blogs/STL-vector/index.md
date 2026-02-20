# C++ vector 学习笔记

> `#include <vector>` 是 C++ STL 中最常用的容器之一，通常用来代替数组。

## 为什么使用 vector？

- **静态数组**：`int a[6]` 大小固定，编译时确定
- **动态数组**：需要自己用 `new/malloc` 开辟，并手动维护内存
- **vector**：STL 封装好的动态数组，自动管理内存，安全高效

---

## 一、构造函数与初始化

| 方式 | 代码示例 | 说明 |
|------|----------|------|
| 默认构造 | `vector<int> v;` | 创建空 vector |
| 拷贝构造 | `vector<int> v2(v1);` | 用另一个 vector 初始化 |
| 区间构造 | `vector<int> v3(v1.begin(), v1.end());` | 用迭代器区间初始化 |
| 填充构造 | `vector<int> v4(10, 100);` | n 个相同元素 |
| 列表初始化 | `vector<int> v5 = {1, 2, 3};` | C++11 列表初始化 |

```cpp
// 默认构造
vector<int> v1;                     // 空 vector

// 拷贝构造
vector<int> v2(v1);                 // 拷贝 v1

// 区间构造（左闭右开）
vector<int> v3(v1.begin(), v1.end());  // [begin, end)

// n 个 elem 构造
vector<int> v4(10, 100);            // 10 个 100

// C++11 列表初始化
vector<int> v5 = {1, 2, 3, 4, 5};
vector<string> v6 = {"hello", "world"};
```

---

## 二、赋值操作

| 方法 | 代码示例 | 说明 |
|------|----------|------|
| `=` 赋值 | `v2 = v1;` | 拷贝赋值 |
| `assign()` 区间 | `v3.assign(v1.begin(), v1.end());` | 用迭代器区间赋值 |
| `assign()` 填充 | `v3.assign(10, 100);` | n 个相同元素 |

```cpp
vector<int> v1 = {1, 2, 3, 4, 5};
vector<int> v2, v3;

// 直接赋值
v2 = v1;                           // v2 = {1, 2, 3, 4, 5}

// assign() 区间赋值
v3.assign(v1.begin(), v1.end());   // v3 = {1, 2, 3, 4, 5}
v3.assign(v1.begin(), v1.begin() + 3);  // v3 = {1, 2, 3}

// assign() 填充赋值
v3.assign(10, 100);                // v3 = {100, 100, ..., 100} (10个)
```

---

## 三、容量与大小操作

| 方法 | 说明 |
|------|------|
| `empty()` | 判断容器是否为空，返回 `true/false` |
| `size()` | 返回容器中元素的个数 |
| `capacity()` | 返回容器的容量（已分配的内存空间） |
| `resize(num)` | 重新指定容器长度，变长补默认值，变短删除元素 |
| `resize(num, elem)` | 重新指定长度，变长补 elem |
| `reserve(n)` | 预留 n 个长度的空间（不改变 size） |

### size 与 capacity 的区别

```cpp
vector<int> v;
v.reserve(10);                     // capacity = 10, size = 0

v.push_back(1);                    // capacity = 10, size = 1
v.push_back(2);
v.push_back(3);

// capacity: 容器当前分配的存储空间大小
// size: 实际存储的元素个数
```

### resize 与 reserve 的区别

```cpp
vector<int> v;

// resize(): 改变 size，新位置可访问
v.resize(5);                       // v = {0, 0, 0, 0, 0}
v.resize(3, 100);                 // v = {0, 0, 0} (删除后2个元素)

// reserve(): 只改变 capacity，预留空间不初始化
v.reserve(10);                     // size 不变，capacity 增大
// 预留位置不可访问，直到 push_back 或 resize
```

---

## 四、插入与删除

| 方法 | 代码示例 | 说明 |
|------|----------|------|
| `push_back()` | `v.push_back(1);` | 尾部插入一个元素 |
| `pop_back()` | `v.pop_back();` | 删除最后一个元素 |
| `insert()` | `v.insert(v.begin(), 1);` | 在指定位置插入 |
| `erase()` | `v.erase(v.begin());` | 删除指定位置元素 |
| `clear()` | `v.clear();` | 清空所有元素 |

```cpp
vector<int> v = {1, 2, 3, 4, 5};

// push_back() - 尾插
v.push_back(6);                    // {1, 2, 3, 4, 5, 6}

// pop_back() - 删除最后一个元素
v.pop_back();                      // {1, 2, 3, 4, 5}

// insert() - 在指定位置插入（参数是迭代器，不是下标）
v.insert(v.begin(), 0);            // {0, 1, 2, 3, 4, 5}
v.insert(v.begin() + 2, 100);     // {0, 1, 100, 2, 3, 4, 5}
v.insert(v.end(), 2, 200);         // {0, 1, 100, 2, 3, 4, 5, 200, 200}

// erase() - 删除元素
v.erase(v.begin());                // {1, 100, 2, 3, 4, 5, 200, 200}
v.erase(v.begin() + 1, v.begin() + 3);  // {1, 3, 4, 5, 200, 200}
// 左闭右开区间 [begin+1, begin+3)

// clear() - 清空容器
v.clear();                         // {}
```

---

## 五、数据存取

| 方法 | 说明 |
|------|------|
| `operator[]` | 下标访问（不检查越界） |
| `at()` | 下标访问（检查越界，抛出异常） |
| `front()` | 返回第一个元素 |
| `back()` | 返回最后一个元素 |
| `data()` | 返回指向数组首元素的指针 |

```cpp
vector<int> v = {10, 20, 30, 40, 50};

// [] 运算符访问（不检查越界）
int a = v[0];                      // 10
v[2] = 300;                        // {10, 20, 300, 40, 50}

// at() 访问（检查越界）
try {
    int b = v.at(10);              // 抛出 std::out_of_range 异常
} catch (const out_of_range& e) {
    cerr << e.what() << endl;
}

// front() 和 back()
int first = v.front();             // 10
int last = v.back();               // 50

// data() - 获取原始指针
int* ptr = v.data();              // 指向内部数组
```

---

## 六、遍历方式

### 方式1：下标遍历

```cpp
vector<int> v = {1, 2, 3, 4, 5};

for (size_t i = 0; i < v.size(); i++) {
    cout << v[i] << " ";
}
```

### 方式2：迭代器遍历

```cpp
vector<int> v = {1, 2, 3, 4, 5};

vector<int>::iterator it = v.begin();
vector<int>::iterator itEnd = v.end();

// begin() 指向第一个元素
// end() 指向最后一个元素的下一位

while (it != itEnd) {
    cout << *it << " ";
    ++it;
}

// 或者使用 C++11 范围 for
for (vector<int>::iterator it = v.begin(); it != v.end(); ++it) {
    cout << *it << " ";
}
```

### 方式3：for_each 算法

```cpp
#include <algorithm>

vector<int> v = {1, 2, 3, 4, 5};

for_each(v.begin(), v.end(), myPrint);

void myPrint(int val) {
    cout << val << " ";
}

// 或者使用 lambda 表达式
for_each(v.begin(), v.end(), [](int val) {
    cout << val << " ";
});
```

### 方式4：范围 for 循环（C++11）

```cpp
vector<int> v = {1, 2, 3, 4, 5};

for (int val : v) {
    cout << val << " ";
}

// 使用引用修改元素
for (int& val : v) {
    val *= 2;                       // 每个元素乘以2
}
```

---

## 七、容器互换与收缩内存

### swap() - 交换两个容器

```cpp
vector<int> v1 = {1, 2, 3};
vector<int> v2 = {4, 5};

v1.swap(v2);                       // v1 = {4, 5}, v2 = {1, 2, 3}

// 或使用 std::swap
std::swap(v1, v2);
```

### 使用 swap 收缩内存

```cpp
vector<int> v;
for (int i = 0; i < 10000; i++) {
    v.push_back(i);
}

// 此时 size = 10000, capacity 可能为 16384 或更大
v.erase(v.begin() + 5, v.end());  // size = 5, capacity 仍然很大

// 使用 swap 收缩内存到实际大小
vector<int>(v).swap(v);
// 或 C++11: v.shrink_to_fit();

// 现在 size = 5, capacity = 5
```

**原理**：创建一个临时 vector 拷贝当前 vector（临时对象只分配实际需要的空间），然后交换当前 vector 和临时对象，临时对象析构时释放多余内存。

---

## 八、二维 vector（容器嵌套）

```cpp
// 创建二维 vector
vector<vector<int>> v;

// 创建内层 vector
vector<int> v1 = {1, 2, 3};
vector<int> v2 = {4, 5, 6};
vector<int> v3 = {7, 8, 9};

// 将内层 vector 添加到外层
v.push_back(v1);
v.push_back(v2);
v.push_back(v3);

// 访问元素
int val = v[1][2];                 // 6 (第二行第三列)

// 遍历二维 vector
for (vector<vector<int>>::iterator it = v.begin(); it != v.end(); ++it) {
    for (vector<int>::iterator vit = (*it).begin(); vit != (*it).end(); ++vit) {
        cout << *vit << " ";
    }
    cout << endl;
}

// 使用范围 for 遍历（更简洁）
for (const auto& row : v) {
    for (const auto& elem : row) {
        cout << elem << " ";
    }
    cout << endl;
}

// 指定大小初始化二维 vector（3行4列，初始化为0）
vector<vector<int>> matrix(3, vector<int>(4, 0));
```

---

## 九、排序与查找

```cpp
#include <algorithm>

vector<int> v = {5, 2, 8, 1, 9, 3};

// sort() - 排序（默认升序）
sort(v.begin(), v.end());          // {1, 2, 3, 5, 8, 9}

// 降序排序
sort(v.begin(), v.end(), greater<int>());

// reverse() - 反转
reverse(v.begin(), v.end());

// find() - 查找
auto it = find(v.begin(), v.end(), 5);
if (it != v.end()) {
    cout << "Found at index: " << (it - v.begin()) << endl;
}

// count() - 统计出现次数
int cnt = count(v.begin(), v.end(), 5);

// unique() - 去重（需要先排序）
sort(v.begin(), v.end());
auto last = unique(v.begin(), v.end());
v.erase(last, v.end());
```

---

## 十、其他常用操作

```cpp
vector<int> v = {1, 2, 3, 4, 5};

// emplace_back() - 原地构造元素（比 push_back 更高效）
v.emplace_back(6);

// emplace() - 原地插入
v.emplace(v.begin(), 0);

// max_size() - 返回可容纳的最大元素数
size_t max = v.max_size();

// shrink_to_fit() - 请求移除未使用的容量（C++11）
v.shrink_to_fit();

// 判断元素是否存在
bool contains(const vector<int>& v, int target) {
    return find(v.begin(), v.end(), target) != v.end();
}
```

---

## 十一、vector 作为函数参数

### 值传递（不推荐，会拷贝整个容器）

```cpp
void func(vector<int> v) {          // 会发生拷贝
    // ...
}
```

### 引用传递（推荐）

```cpp
void func(vector<int>& v) {         // 可修改原容器
    // ...
}

void func(const vector<int>& v) {   // 只读，不修改原容器
    // ...
}
```

---

## 十二、注意事项

1. **插入/删除导致迭代器失效**：`insert` 和 `erase` 之后，之前获取的迭代器可能失效
2. **vector 存储地址是连续的**：可以使用 `data()` 获取原始指针
3. **频繁插入删除考虑 list**：如果需要在中间频繁操作，考虑使用 `list` 或 `deque`
4. **预留空间提高性能**：如果知道大概需要多少元素，使用 `reserve()` 预分配
5. **下标访问不检查越界**：使用 `[]` 时要确保下标有效，或使用 `at()` 获得安全检查
6. **swap 收缩内存技巧**：大容器删除大量元素后，使用 `vector(v).swap(v)` 收缩内存

---

## 十三、常用函数速查表

| 分类 | 函数 | 功能 |
|------|------|------|
| 构造 | `vector<T>()` | 默认构造 |
| | `vector<T>(n, val)` | n 个 val |
| | `vector<T>(begin, end)` | 迭代器区间构造 |
| 赋值 | `operator=` | 赋值 |
| | `assign()` | 赋值 |
| 容量 | `size()` | 元素个数 |
| | `empty()` | 是否为空 |
| | `capacity()` | 容量 |
| | `reserve()` | 预留空间 |
| | `resize()` | 调整大小 |
| 访问 | `operator[]` | 下标访问 |
| | `at()` | 带检查的下标访问 |
| | `front()`, `back()` | 首尾元素 |
| | `data()` | 获取原始指针 |
| 修改 | `push_back()` | 尾部插入 |
| | `pop_back()` | 删除尾部 |
| | `insert()` | 插入 |
| | `erase()` | 删除 |
| | `clear()` | 清空 |
| | `swap()` | 交换容器 |
| 迭代器 | `begin()`, `end()` | 正向迭代器 |
| | `rbegin()`, `rend()` | 反向迭代器 |
| C++11 | `emplace_back()` | 原地构造插入 |
| | `shrink_to_fit()` | 收缩容量 |
| 算法 | `sort()` | 排序 |
| | `find()` | 查找 |
| | `reverse()` | 反转 |
