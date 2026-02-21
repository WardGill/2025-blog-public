# C++ map 学习笔记

> `#include <map>` 是 C++ STL 中提供的映射容器。

---

## 一、pair 对组

`pair` 是将两个数据组合成一组数据，常用于 map 中存储键值对。

### pair 的构造

| 方式 | 代码示例 | 说明 |
|------|----------|------|
| 直接构造 | `pair<int, int> p(v1, v2);` | 创建 pair |
| make_pair | `auto p = make_pair(v1, v2);` | 创建 pair（自动推导类型） |
| C++11 列表初始化 | `pair<int, int> p = {v1, v2};` | 列表初始化 |

### pair 的访问

```cpp
// 创建 pair
pair<string, int> p1("Alice", 25);
auto p2 = make_pair("Bob", 30);

// 访问成员
cout << p1.first << endl;          // "Alice" (key)
cout << p1.second << endl;         // 25 (value)

// 修改成员
p1.second = 26;                    // 修改值
```

### pair 的比较

```cpp
pair<int, int> p1 = {1, 2};
pair<int, int> p2 = {1, 3};
pair<int, int> p3 = {2, 1};

// 先比较 first，再比较 second
cout << (p1 < p2) << endl;       // 1 (true, 1==1, 2<3)
cout << (p1 < p3) << endl;       // 1 (true, 1<2)
cout << (p2 == p1) << endl;      // 0 (false)
```

---

## 二、什么是 map？

**map** 是一种**键值对（Key-Value）映射**的关联容器，所有元素都是 pair。

### map 的结构

```
map<string, int> m = {
    {"Bob", 30},
    {"Alice", 25},
    {"Charlie", 20}
};

按照 key 自动排序：
    ┌─────────────┬─────┐
    │  "Alice"    │  25  │
    ├─────────────┼─────┤
    │  "Bob"      │  30  │
    ├─────────────┼─────┤
    │  "Charlie"  │  20  │
    └─────────────┴─────┘
      (key)      (value)
```

### map 的元素结构

每个元素都是一个 `pair<Key, Value>`：
- **first**：键（key），用于排序和查找
- **second**：值（value），存储实际数据

### map 与 multimap 的区别

| 特性 | map | multimap |
|------|-----|----------|
| key 唯一性 | ✅ 不允许重复 key | ❌ 允许重复 key |
| 自动排序 | ✅ 按 key 排序 | ✅ 按 key 排序 |
| 底层实现 | 红黑树 | 红黑树 |
| 访问方式 | `m[key]` 或 `m.at(key)` | 只能通过迭代器 |
| 插入时间复杂度 | O(log n) | O(log n) |
| 查找时间复杂度 | O(log n) | O(log n) |

### map 的特点

1. **键值对存储**：每个元素都是 pair
2. **key 唯一**：相同的 key 只会保留一个（map）
3. **自动排序**：元素按照 key 自动排序
4. **高效查找**：基于红黑树，查找时间为 O(log n)

---

## 三、构造函数与初始化

| 方式 | 代码示例 | 说明 |
|------|----------|------|
| 默认构造 | `map<int, int> m;` | 创建空 map |
| 拷贝构造 | `map<int, int> m2(m1);` | 用另一个 map 初始化 |
| 列表初始化 | `map<int, int> m = {{1,2}, {3,4}};` | C++11 列表初始化 |
| 区间构造 | `map<int, int> m(v.begin(), v.end());` | 用迭代器区间初始化 |

```cpp
#include <map>
using namespace std;

// 默认构造
map<int, string> m;

// 列表初始化
map<int, string> m1 = {
    {1, "one"},
    {3, "three"},
    {2, "two"}
};
// 自动排序后: {1:"one", 2:"two", 3:"three"}

// 拷贝构造
map<int, string> m2(m1);

// multimap -允许重复 key
multimap<int, string> mm = {
    {1, "one"},
    {1, "uno"},
    {2, "two"}
};
```

---

## 四、赋值与交换

| 方法 | 代码示例 | 说明 |
|------|----------|------|
| `operator=` | `m1 = m2;` | 拷贝赋值 |
| `swap()` | `m1.swap(m2);` | 交换两个 map |

```cpp
map<int, string> m1 = {{1, "one"}, {2, "two"}};
map<int, string> m2;

// 直接赋值
m2 = m1;                         // m2 = {1:"one", 2:"two"}

// swap() 交换
map<int, string> m3 = {{3, "three"}, {4, "four"}};
m1.swap(m3);                     // m1 = {3:"three", 4:"four"}
```

---

## 五、大小操作

| 方法 | 说明 |
|------|------|
| `empty()` | 判断 map 是否为空，返回 `true/false` |
| `size()` | 返回 map 中元素的个数 |

```cpp
map<int, string> m = {{1, "one"}, {2, "two"}, {3, "three"}};

cout << m.empty() << endl;         // 0 (false)
cout << m.size() << endl;           // 3
```

---

## 六、插入与删除

| 方法 | 代码示例 | 说明 |
|------|----------|------|
| `insert()` | `m.insert(pair);` | 插入元素 |
| `erase(pos)` | `m.erase(it);` | 删除迭代器指向的元素 |
| `erase(begin, end)` | `m.erase(b, e);` | 删除区间元素 |
| `erase(key)` | `m.erase(key);` | 删除指定 key 的元素 |
| `clear()` | `m.clear();` | 清空 map |

```cpp
map<int, string> m;

// insert() - 插入元素
m.insert({1, "one"});             // 方式1：使用花括号
m.insert(make_pair(2, "two"));     // 方式2：使用 make_pair
m.insert(pair<int, string>(3, "three"));  // 方式3：直接构造

// insert() 返回值：pair<iterator, bool>
auto result = m.insert({4, "four"});
if (result.second) {
    cout << "插入成功" << endl;
}

// [] 运算符插入（如果不存在则插入）
m[5] = "five";                   // 直接赋值

// erase(key) - 删除指定 key
m.erase(2);                       // 删除 key=2 的元素

// erase(pos) - 删除迭代器指向的元素
auto it = m.begin();
m.erase(it);                      // 删除第一个元素

// erase(begin, end) - 删除区间
m.erase(m.begin(), next(m.begin(), 2));

// clear() - 清空
m.clear();                        // m = {}
```

---

## 七、查找与访问

| 方法 | 说明 |
|------|------|
| `find(key)` | 查找 key，存在返回迭代器，不存在返回 `end()` |
| `count(key)` | 统计 key 的个数（map 返回 0 或 1） |
| `operator[]` | 访问或创建元素 |
| `at()` | 访问元素（越界抛出异常） |
| `lower_bound(key)` | 返回第一个 key ≥ 指定值的迭代器 |
| `upper_bound(key)` | 返回第一个 key > 指定值的迭代器 |

```cpp
map<int, string> m = {{1, "one"}, {2, "two"}, {3, "three"}};

// [] 运算符 - 访问元素
cout << m[2] << endl;            // "two"
m[4] = "four";                   // key 不存在，自动插入

// at() - 访问元素（带边界检查）
try {
    cout << m.at(2) << endl;      // "two"
    cout << m.at(10) << endl;     // 抛出异常
} catch (const out_of_range& e) {
    cerr << e.what() << endl;
}

// find() - 查找元素
auto it = m.find(2);
if (it != m.end()) {
    cout << "key: " << it->first << ", value: " << it->second << endl;
}

// count() - 统计 key 个数
cout << m.count(2) << endl;      // 1 (存在)
cout << m.count(10) << endl;     // 0 (不存在)

// multimap 的 count()
multimap<int, string> mm = {{1, "one"}, {1, "uno"}};
cout << mm.count(1) << endl;     // 2
```

---

## 八、遍历 map

```cpp
map<int, string> m = {{3, "three"}, {1, "one"}, {2, "two"}};
// 自动排序: {1:"one", 2:"two", 3:"three"}

// 方式1：迭代器遍历
for (map<int, string>::iterator it = m.begin(); it != m.end(); ++it) {
    cout << it->first << ": " << it->second << endl;
}

// 方式2：范围 for（C++11）
for (const auto& p : m) {
    cout << p.first << ": " << p.second << endl;
}

// 方式3：结构化绑定（C++17）
for (const auto& [key, value] : m) {
    cout << key << ": " << value << endl;
}

// 方式4：只遍历 key
for (const auto& p : m) {
    cout << p.first << endl;       // 只输出 key
}

// 方式5：只遍历 value
for (const auto& p : m) {
    cout << p.second << endl;      // 只输出 value
}
```

---

## 九、自定义比较

```cpp
// 降序排序的 map
map<int, string, greater<int>> m = {
    {1, "one"},
    {3, "three"},
    {2, "two"}
};
// m = {3:"three", 2:"two", 1:"one"}

// 自定义 key 类型
struct Person {
    string name;
    int age;

    // 重载 < 运算符用于排序
    bool operator<(const Person& other) const {
        return age < other.age;     // 按年龄升序
    }
};

map<Person, string> people = {
    {{"Alice", 25}, "Engineer"},
    {{"Bob", 30}, "Manager"},
    {{"Charlie", 20}, "Intern"}
};

for (const auto& [p, role] : people) {
    cout << p.name << "(" << p.age << "): " << role << endl;
}
// 输出（按年龄排序）:
// Charlie(20): Intern
// Alice(25): Engineer
// Bob(30): Manager
```

---

## 十、map 的应用场景

### 1. 词频统计

```cpp
map<string, int> wordCount;
string text = "hello world hello cpp world hello";

stringstream ss(text);
string word;

while (ss >> word) {
    wordCount[word]++;             // [] 运算符自动初始化为 0
}

for (const auto& [word, count] : wordCount) {
    cout << word << ": " << count << endl;
}
// 输出:
// cpp: 1
// hello: 3
// world: 2
```

### 2. 配置文件存储

```cpp
class Config {
private:
    map<string, string> data;

public:
    void set(const string& key, const string& value) {
        data[key] = value;
    }

    string get(const string& key, const string& defaultValue = "") {
        auto it = data.find(key);
        return (it != data.end()) ? it->second : defaultValue;
    }

    bool has(const string& key) {
        return data.count(key) > 0;
    }
};

Config config;
config.set("host", "localhost");
config.set("port", "8080");

cout << config.get("host") << endl;        // localhost
cout << config.get("port") << endl;        // 8080
cout << config.get("timeout", "30") << endl; // 30 (默认值)
```

### 3. 缓存实现

```cpp
class SimpleCache {
private:
    map<string, string> cache;
    size_t maxSize;

public:
    SimpleCache(size_t size) : maxSize(size) {}

    void put(const string& key, const string& value) {
        if (cache.size() >= maxSize) {
            // 简单策略：删除第一个元素
            cache.erase(cache.begin());
        }
        cache[key] = value;
    }

    string get(const string& key) {
        auto it = cache.find(key);
        if (it != cache.end()) {
            return it->second;
        }
        return "";
    }
};
```

### 4. 两数之和

```cpp
vector<int> twoSum(vector<int>& nums, int target) {
    map<int, int> map;  // value -> index

    for (int i = 0; i < nums.size(); i++) {
        int complement = target - nums[i];
        if (map.count(complement)) {
            return {map[complement], i};
        }
        map[nums[i]] = i;
    }

    return {};
}

// nums = [2, 7, 11, 15], target = 9
// 结果: [0, 1] (2 + 7 = 9)
```

---

## 十一、unordered_map

`unordered_map` 是基于哈希表实现的映射容器，查找更快但无序。

### map 与 unordered_map 的区别

| 特性 | map | unordered_map |
|------|-----|---------------|
| 底层实现 | 红黑树 | 哈希表 |
| 元素顺序 | 有序 | 无序 |
| 查找时间复杂度 | O(log n) | O(1) 平均 |
| 插入时间复杂度 | O(log n) | O(1) 平均 |
| key 要求 | 支持 `<` 运算符 | 支持哈希函数 |

```cpp
#include <unordered_map>

unordered_map<string, int> scores;
scores["Alice"] = 95;
scores["Bob"] = 87;
scores["Charlie"] = 92;

// 遍历（顺序不确定）
for (const auto& [name, score] : scores) {
    cout << name << ": " << score << endl;
}
```

---

## 十二、注意事项

1. **key 不可修改**：map 中的 key 是 const 的，不能直接修改
2. **[] 会自动插入**：使用 `m[key]` 访问不存在的 key 会自动插入默认值
3. **at() 带检查**：`m.at(key)` 会检查边界，不存在则抛出异常
4. **multimap 没有 []**：multimap 不能用 `[]` 运算符访问
5. **迭代器指向 pair**：`it->first` 访问 key，`it->second` 访问 value

---

## 十三、常用函数速速查表

### pair

| 方法 | 说明 |
|------|------|
| `make_pair()` | 创建 pair |
| `first`, `second` | 访问成员 |

### map

| 分类 | 函数 | 功能 |
|------|------|------|
| 构造 | `map<K, V>()` | 默认构造 |
| | `map<K, V>(begin, end)` | 区间构造 |
| 赋值 | `operator=` | 赋值 |
| | `swap()` | 交换 |
| 大小 | `size()` | 元素个数 |
| | `empty()` | 是否为空 |
| 插入 | `insert()` | 插入元素 |
| | `operator[]` | 访问或插入 |
| 删除 | `erase()` | 删除元素 |
| | `clear()` | 清空 |
| 访问 | `at()` | 访问元素（带检查） |
| | `operator[]` | 访问元素 |
| 查找 | `find()` | 查找元素 |
| | `count()` | 统计个数 |
| | `lower_bound()` | 第一个 key ≥ 指定值 |
| | `upper_bound()` | 第一个 key > 指定值 |
| 迭代器 | `begin()`, `end()` | 正向迭代器 |
| | `rbegin()`, `rend()` | 反向迭代器 |

---

## 十四、力扣相关题目

| 题目 | 难度 | 考点 |
|------|------|------|
| [1. 两数之和](https://leetcode.cn/problems/two-sum/) | 简单 | map |
| [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/) | 中等 | unordered_map / 滑动窗口 |
| [169. 多数元素](https://leetcode.cn/problems/majority-element/) | 简单 | unordered_map |
| [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/) | 中等 | unordered_map / set |
| [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/) | 中等 | unordered_map + 优先队列 |
| [496. 下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/) | 简单 | map / 栈 |
| [560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/) | 中等 | unordered_map |
| [763. 划分字母区间](https://leetcode.cn/problems/partition-labels/) | 中等 | map |
| [451. 字符频率排序](https://leetcode.cn/problems/sort-characters-by-frequency/) | 中等 | unordered_map |
