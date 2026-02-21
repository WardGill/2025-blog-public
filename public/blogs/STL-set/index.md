# C++ set 学习笔记

> `#include <set>` 是 C++ STL 中提供的集合容器。

---

## 什么是集合？

**集合** 是一种**自动排序且元素唯一**的关联容器。

### set 与 multiset 的区别

| 特性 | set | multiset |
|------|-----|----------|
| 元素唯一性 | ✅ 不允许重复元素 | ❌ 允许重复元素 |
| 自动排序 | ✅ 是 | ✅ 是 |
| 底层实现 | 红黑树 | 红黑树 |
| 插入时间复杂度 | O(log n) | O(log n) |
| 查找时间复杂度 | O(log n) | O(log n) |

### 集合的特点

```
set<int> st = {5, 2, 8, 1, 3};

插入后自动排序：
    ┌───┬───┬───┬───┬───┐
    │ 1 │ 2 │ 3 │ 5 │ 8 │
    └───┴───┴───┴───┴───┘
```

1. **自动排序**：插入元素时自动按照指定规则排序
2. **元素唯一**：相同元素只会保留一个（set）
3. **高效查找**：基于红黑树，查找时间为 O(log n)
4. **不能修改元素**：只能插入和删除，不能直接修改元素值

### set 的基本操作

| 操作 | 说明 |
|------|------|
| 插入 `insert()` | 插入元素，自动排序 |
| 删除 `erase()` | 删除指定元素 |
| 查找 `find()` | 查找元素是否存在 |
| 统计 `count()` | 统计元素个数 |
| 遍历 | 迭代器遍历（有序） |

---

## 一、构造函数与初始化

| 方式 | 代码示例 | 说明 |
|------|----------|------|
| 默认构造 | `set<int> st;` | 创建空集合 |
| 拷贝构造 | `set<int> st2(st1);` | 用另一个 set 初始化 |
| 列区初始化 | `set<int> st = {3, 1, 2};` | C++11 列表初始化 |
| 区间构造 | `set<int> st(v.begin(), v.end());` | 用迭代器区间初始化 |

```cpp
#include <set>
using namespace std;

// 默认构造
set<int> st;                      // 空集合

// 列表初始化
set<int> st1 = {5, 2, 8, 1, 3};
// 自动排序后: {1, 2, 3, 5, 8}

// 拷贝构造
set<int> st2(st1);               // st2 = {1, 2, 3, 5, 8}

// 区间构造
vector<int> v = {10, 20, 30, 20};
set<int> st3(v.begin(), v.end());  // st3 = {10, 20, 30} (重复元素被去除)

// multiset - 允许重复元素
multiset<int> mst = {1, 2, 2, 3};
// mst = {1, 2, 2, 3}
```

---

## 二、赋值与交换

| 方法 | 代码示例 | 说明 |
|------|----------|------|
| `operator=` | `st1 = st2;` | 拷贝赋值 |
| `swap()` | `st1.swap(st2);` | 交换两个集合 |

```cpp
set<int> st1 = {1, 2, 3};
set<int> st2;

// 直接赋值
st2 = st1;                        // st2 = {1, 2, 3}

// swap() 交换
set<int> st3 = {4, 5, 6};
st1.swap(st3);                     // st1 = {4, 5, 6}, st3 = {1, 2, 3}
```

---

## 三、大小操作

| 方法 | 说明 |
|------|------|
| `empty()` | 判断集合是否为空，返回 `true/false` |
| `size()` | 返回集合中元素的个数 |

```cpp
set<int> st = {1, 2, 3, 4, 5};

cout << st.empty() << endl;       // 0 (false)
cout << st.size() << endl;         // 5

// multiset 可以有重复元素
multiset<int> mst = {1, 2, 2, 3};
cout << mst.size() << endl;        // 4
```

---

## 四、插入与删除

| 方法 | 代码示例 | 说明 |
|------|----------|------|
| `insert()` | `st.insert(elem);` | 插入元素 |
| `erase(pos)` | `st.erase(it);` | 删除迭代器指向的元素 |
| `erase(begin, end)` | `st.erase(b, e);` | 删除区间元素 |
| `erase(elem)` | `st.erase(elem);` | 删除指定元素 |
| `clear()` | `st.clear();` | 清空集合 |

```cpp
set<int> st = {2, 4, 6};

// insert() - 插入元素
st.insert(1);                      // st = {1, 2, 4, 6}
st.insert(3);                      // st = {1, 2, 3, 4, 6}
st.insert(2);                      // st = {1, 2, 3, 4, 6} (重复元素不插入)

// insert() 返回值：pair<iterator, bool>
auto result = st.insert(5);
if (result.second) {
    cout << "插入成功" << endl;
} else {
    cout << "元素已存在" << endl;
}

// erase(elem) - 删除指定元素
st.erase(2);                       // st = {1, 3, 4, 5, 6}

// erase(pos) - 删除迭代器指向的元素
auto it = st.begin();
advance(it, 2);                     // 指向 4
st.erase(it);                      // st = {1, 3, 5, 6}

// erase(begin, end) - 删除区间
st.erase(st.begin(), st.begin() + 2);  // st = {5, 6}

// clear() - 清空
st.clear();                        // st = {}
```

---

## 五、查找与统计

| 方法 | 说明 |
|------|------|
| `find(key)` | 查找元素，存在返回迭代器，不存在返回 `end()` |
| `count(key)` | 统计元素个数（set 返回 0 或 1） |
| `lower_bound(key)` | 返回第一个 ≥ key 的元素迭代器 |
| `upper_bound(key)` | 返回第一个 > key 的元素迭代器 |
| `equal_range(key)` | 返回等于 key 的元素范围 |

```cpp
set<int> st = {1, 3, 5, 7, 9};

// find() - 查找元素
auto it = st.find(5);
if (it != st.end()) {
    cout << "找到元素: " << *it << endl;
} else {
    cout << "元素不存在" << endl;
}

// count() - 统计元素个数
cout << st.count(5) << endl;      // 1 (存在)
cout << st.count(2) << endl;      // 0 (不存在)

// multiset 的 count()
multiset<int> mst = {1, 2, 2, 2, 3};
cout << mst.count(2) << endl;     // 3

// lower_bound() - 第一个 >= key 的元素
auto lb = st.lower_bound(5);
cout << *lb << endl;               // 5

auto lb2 = st.lower_bound(4);
cout << *lb2 << endl;              // 5 (第一个 >= 4 的元素)

// upper_bound() - 第一个 > key 的元素
auto ub = st.upper_bound(5);
cout << *ub << endl;               // 7

// equal_range() - 等于 key 的元素范围
auto range = st.equal_range(5);
// range.first: lower_bound
// range.second: upper_bound
```

---

## 六、遍历集合

```cpp
set<int> st = {3, 1, 4, 1, 5, 9};  // 自动排序: {1, 3, 4, 5, 9}

// 方式1：迭代器遍历
for (set<int>::iterator it = st.begin(); it != st.end(); ++it) {
    cout << *it << " ";            // 输出: 1 3 4 5 9
}

// 方式2：范围 for（C++11）
for (int val : st) {
    cout << val << " ";
}

//方式3：反向遍历
for (auto it = st.rbegin(); it != st.rend(); ++it) {
    cout << *it << " ";            // 输出: 9 5 4 3 1
}
```

---

## 七、自定义比较

```cpp
// 降序排序的 set
set<int, greater<int>> st = {1, 3, 2, 5};
// st = {5, 3, 2, 1}

// 自定义类型的 set
struct Person {
    string name;
    int age;
};

// 按年龄升序
struct PersonCompare {
    bool operator()(const Person& a, const Person& b) const {
        return a.age < b.age;
    }
};

set<Person, PersonCompare> people = {
    {"Alice", 25},
    {"Bob", 30},
    {"Charlie", 20}
};

// 按年龄排序遍历
for (const auto& p : people) {
    cout << p.name << ": " << p.age << endl;
}
// 输出:
// Charlie: 20
// Alice: 25
// Bob: 30
```

---

## 八、集合运算

```cpp
#include <algorithm>

// 交集
set<int> setA = {1, 2, 3, 4, 5};
set<int> setB = {3, 4, 5, 6, 7};

set<int> intersection;
set_intersection(
    setA.begin(), setA.end(),
    setB.begin(), setB.end(),
    inserter(intersection, intersection.begin())
);
// intersection = {3, 4, 5}

// 并集
set<int> unionSet;
set_union(
    setA.begin(), setA.end(),
    setB.begin(), setB.end(),
    inserter(unionSet, unionSet.begin())
);
// unionSet = {1, 2, 3, 4, 5, 6, 7}

// 差集 (A - B)
set<int> difference;
set_difference(
    setA.begin(), setA.end(),
    setB.begin(), setB.end(),
    inserter(difference, difference.begin())
);
// difference = {1, 2}
```

---

## 九、set 的应用场景

### 1. 去重

```cpp
vector<int> nums = {1, 2, 2, 3, 3, 3, 4};

// 方法1：使用 set 去重
set<int> s(nums.begin(), nums.end());
vector<int> result(s.begin(), s.end());
// result = {1, 2, 3, 4}

// 方法2：保持原顺序的去重
vector<int> nums2 = {1, 2, 2, 3, 3, 3, 4};
set<int> seen;
set<int> result2;
for (int num : nums2) {
    if (seen.insert(num).second) {
        result2.push_back(num);
    }
}
// result2 = {1, 2, 3, 4}
```

### 2. 判断元素是否存在

```cpp
set<string> dictionary = {"hello", "world", "cpp"};

string word = "hello";
if (dictionary.find(word) != dictionary.end()) {
    cout << "单词在字典中" << endl;
}

// 或使用 count()
if (dictionary.count(word)) {
    cout << "单词在字典中" << endl;
}
```

### 3. 找出两个数组的交集

```cpp
vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
    set<int> s1(nums1.begin(), nums1.end());
    set<int> s2(nums2.begin(), nums2.end());

    vector<int> result;
    for (int num : s1) {
        if (s2.count(num)) {
            result.push_back(num);
        }
    }
    return result;
}

// nums1 = {1, 2, 2, 1}, nums2 = {2, 2}
// 结果: {2}
```

### 4. 找出第 K 小的数

```cpp
int findKthSmallest(vector<int>& nums, int k) {
    multiset<int> s(nums.begin(), nums.end());

    auto it = s.begin();
    advance(it, k - 1);
    return *it;
}
```

---

## 十、注意事项

1. **元素不可修改**：set 中的元素是 const 的，不能直接修改
2. **不能通过迭代器修改**：即使有迭代器，`*it = value` 也是非法的
3. **查找使用 `find()`**：不要用循环遍历查找，效率低
4. **插入返回值**：`insert()` 返回 `pair<iterator, bool>`，第二个值表示是否插入成功
5. **自定义比较**：对于自定义类型，需要提供比较函数或重载 `<` 运算符

---

## 十一、常用函数速查表

| 分类 | 函数 | 功能 |
|------|------|------|
| 构造 | `set<T>()` | 默认构造 |
| | `set<T>(begin, end)` | 区间构造 |
| 赋值 | `operator=` | 赋值 |
| | `swap()` | 交换 |
| 大小 | `size()` | 元素个数 |
| | `empty()` | 是否为空 |
| 插入 | `insert()` | 插入元素 |
| 删除 | `erase()` | 删除元素 |
| | `clear()` | 清空 |
| 查找 | `find()` | 查找元素 |
| | `count()` | 统计个数 |
| | `lower_bound()` | 第一个 ≥ key |
| | `upper_bound()` | 第一个 > key |
| | `equal_range()` | 等于 key 的范围 |
| 迭代器 | `begin()`, `end()` | 正向迭代器 |
| | `rbegin()`, `rend()` | 反向迭代器 |

---

## 十二、力扣相关题目

| 题目 | 难度 | 考点 |
|------|------|------|
| [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/) | 简单 | 集合交集 |
| [350. 两个数组的交集 II](https://leetcode.cn/problems/intersection-of-two-arrays-ii/) | 简单 | multiset / 哈希表 |
| [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/) | 中等 | set |
| [217. 存在重复元素](https://leetcode.cn/problems/contains-duplicate/) | 简单 | set 去重 |
| [414. 第三大的数](https://leetcode.cn/problems/third-maximum-number/) | 简单 | set |
| [720. 词典中最长的单词](https://leetcode.cn/problems/longest-word-in-dictionary/) | 中等 | set |
| [136. 只出现一次的数字](https://leetcode.cn/problems/single-number/) | 简单 | set / 位运算 |
| [380. O(1) 时间插入、删除和获取随机元素](https://leetcode.cn/problems/insert-delete-getrandom-o1/) | 中等 | set + vector |
| [740. 删除并获得点数](https://leetcode.cn/problems/delete-and-earn/) | 中等 | set / 动态规划 |
