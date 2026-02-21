# C++ list 学习笔记

> `#include <list>` 是 C++ STL 中提供的双向链表容器。

---

## 什么是链表？

**链表** 是一种**非连续存储**的线性数据结构，通过指针将各节点连接起来。

### 链表与数组（vector）的区别

| 特性 | vector（数组） | list（链表） |
|------|---------------|--------------|
| 存储方式 | 连续内存 | 非连续内存 |
| 随机访问 | 支持 `[]` / `at()` | ❌ 不支持 |
| 头部插入/删除 | O(n)，需要移动元素 | O(1) |
| 中间插入/删除 | O(n)，需要移动元素 | O(1) |
| 尾部插入/删除 | 均摊 O(1) | O(1) |
| 内存占用 | 较小 | 较大（每个节点多存指针） |
| 迭代器失效 | 插入/删除可能失效 | 只有删除当前节点才失效 |

### 链表的结构

```
┌───────┐   ┌───────┐   ┌───────┐   ┌───────┐
│ NULL  │←──│  10   │←──│  20   │←──│  30   │
└───────┘   └───────┘   └───────┘   └───────┘
              prev  ↑            prev  ↑
                    │                  │
                    ▼            next   ▼
              ┌───────┐            ┌───────┐
              │  40   │            │  50   │
              └───────┘            └───────┘
                    ↑                  │
                    │            next   ▼
                    └────────────────→ NULL
```

### 链表的特点

1. **非连续存储**：节点在内存中不连续，通过指针连接
2. **双向链表**：C++ `list` 是双向链表，可前向和后向遍历
3. **高效插入删除**：已知位置时插入删除时间为 O(1)
4. **不支持随机访问**：不能通过下标直接访问元素

---

## 一、构造函数与初始化

| 方式 | 代码示例 | 说明 |
|------|----------|------|
| 默认构造 | `list<int> L1;` | 创建空链表 |
| 拷贝构造 | `list<int> L2(L1);` | 用另一个 list 初始化 |
| 区间构造 | `list<int> L3(L1.begin(), L1.end());` | 用迭代器区间初始化 |
| 填充构造 | `list<int> L4(10, 100);` | n 个相同元素 |
| 列表初始化 | `list<int> L5 = {1, 2, 3};` | C++11 列表初始化 |

```cpp
#include <list>
using namespace std;

// 默认构造
list<int> L1;

// 尾插法
L1.push_back(10);
L1.push_back(20);
L1.push_back(30);

// 拷贝构造
list<int> L2(L1);                   // L2 = {10, 20, 30}

// 区间构造（左闭右开）
list<int> L3(L1.begin(), L1.end());  // L3 = {10, 20, 30}

// n 个 elem 构造
list<int> L4(10, 100);               // L4 = {100, 100, ..., 100} (10个)

// C++11 列表初始化
list<int> L5 = {1, 2, 3, 4, 5};
```

---

## 二、赋值与交换

| 方法 | 代码示例 | 说明 |
|------|----------|------|
| `operator=` | `L4 = L3;` | 拷贝赋值 |
| `assign()` 区间 | `L3.assign(L2.begin(), L2.end());` | 用迭代器区间赋值 |
| `assign()` 填充 | `L3.assign(10, 100);` | n 个相同元素 |
| `swap()` | `L1.swap(L2);` | 交换两个链表 |

```cpp
list<int> L1 = {1, 2, 3};
list<int> L2, L3;

// 直接赋值
L2 = L1;                           // L2 = {1, 2, 3}

// assign() 区间赋值
L3.assign(L1.begin(), L1.end());   // L3 = {1, 2, 3}
L3.assign(L1.begin(), L1.begin() + 2);  // L3 = {1, 2}

// assign() 填充赋值
L3.assign(10, 100);                // L3 = {100, 100, ..., 100}

// swap() 交换
L1.swap(L2);                       // L1 和 L2 内容互换
```

---

## 三、大小操作

| 方法 | 说明 |
|------|------|
| `empty()` | 判断链表是否为空，返回 `true/false` |
| `size()` | 返回链表中元素的个数 |
| `resize(n)` | 重新指定链表长度，变长补默认值，变短删除元素 |
| `resize(n, elem)` | 重新指定长度，变长补 elem |

```cpp
list<int> L = {1, 2, 3, 4, 5};

cout << L.empty() << endl;         // 0 (false)
cout << L.size() << endl;           // 5

// resize() - 调整大小
L.resize(3);                       // L = {1, 2, 3} (删除后2个元素)
L.resize(5, 100);                   // L = {1, 2, 3, 100, 100}
```

---

## 四、插入与删除

### 链表特有操作

| 方法 | 代码示例 | 说明 |
|------|----------|------|
| `push_front()` | `L.push_front(elem);` | 头部插入 |
| `pop_front()` | `L.pop_front();` | 头部删除 |

```cpp
list<int> L = {2, 3, 4};

// push_front() - 头插
L.push_front(1);                   // L = {1, 2, 3, 4}

// pop_front() - 头删
L.pop_front();                     // L = {2, 3, 4}
```

### 插入操作

| 方法 | 代码示例 | 说明 |
|------|----------|------|
| `insert(pos, elem)` | `L.insert(it, elem);` | 在 pos 位置插入 elem |
| `insert(pos, n, elem)` | `L.insert(it, n, elem);` | 在 pos 位置插入 n 个 elem |
| `insert(pos, begin, end)` | `L.insert(it, b, e);` | 在 pos 位置插入区间元素 |

```cpp
list<int> L = {1, 4, 5};
auto it = L.begin();
advance(it, 1);                     // it 指向 4

// insert() - 在指定位置插入
auto newIt = L.insert(it, 2);       // L = {1, 2, 4, 5}
L.insert(newIt, 3, 100);           // L = {1, 100, 100, 100, 2, 4, 5}
```

### 删除操作

| 方法 | 代码示例 | 说明 |
|------|----------|------|
| `pop_back()` | `L.pop_back();` | 删除最后一个元素 |
| `erase(pos)` | `L.erase(it);` | 删除 pos 位置的元素 |
| `erase(begin, end)` | `L.erase(b, e);` | 删除区间元素 |
| `remove(elem)` | `L.remove(elem);` | 删除所有与 elem 匹配的值 |
| `clear()` | `L.clear();` | 清空链表 |

```cpp
list<int> L = {1, 2, 3, 2, 4, 2};

// pop_back() - 删除最后一个元素
L.pop_back();                      // L = {1, 2, 3, 2, 4}

// erase() - 删除指定位置
auto it = L.begin();
advance(it, 2);                     // it 指向 3
L.erase(it);                        // L = {1, 2, 2, 4}

// erase() - 删除区间
L.erase(L.begin(), L.begin() + 1); // L = {2, 2, 4}

// remove() - 删除所有匹配的值
L.remove(2);                       // L = {4}

// clear() - 清空
L.clear();                         // L = {}
```

---

## 五、数据存取

| 方法 | 说明 |
|------|------|
| `front()` | 返回第一个元素 |
| `back()` | 返回最后一个元素 |

⚠️ **注意**：链表不支持随机访问，不能使用 `[]` 或 `at()`。

```cpp
list<int> L = {10, 20, 30, 40, 50};

// front() - 第一个元素
int first = L.front();             // 10

// back() - 最后一个元素
int last = L.back();               // 50

// ❌ 不支持随机访问
// int val = L[2];  // 编译错误
// int val = L.at(2); // 编译错误
```

---

## 六、迭代器与遍历

### 迭代器类型

| 类型 | 说明 |
|------|------|
| `iterator` | 普通迭代器，支持读写元素 |
| `const_iterator` | 常量迭代器，仅支持读取元素 |
| `reverse_iterator` | 反向迭代器 |

```cpp
list<int> L = {1, 2, 3, 4, 5};

// 普通迭代器
for (list<int>::iterator it = L.begin(); it != L.end(); ++it) {
    cout << *it << " ";            // 输出: 1 2 3 4 5
    *it *= 2;                      // 修改元素
}

// 常量迭代器
for (list<int>::const_iterator it = L.cbegin(); it != L.cend(); ++it) {
    cout << *it << " ";            // 只能读取
}

// 反向迭代器
for (list<int>::reverse_iterator rit = L.rbegin(); rit != L.rend(); ++rit) {
    cout << *rit << " ";          // 输出: 5 4 3 2 1
}

// C++11 范围 for
for (int& val : L) {
    cout << val << " ";
}

// 使用 advance 移动迭代器（链表不支持 it + n）
auto it = L.begin();
advance(it, 3);                     // it 向后移动 3 位
```

---

## 七、反转与排序

### 反转

```cpp
list<int> L = {1, 2, 3, 4, 5};

// reverse() - 反转链表
L.reverse();                       // L = {5, 4, 3, 2, 1}
```

### 排序

⚠️ **注意**：对于不支持随机访问的容器（如 list），不能使用标准算法 `<algorithm>` 中的 `sort()`，必须使用容器成员函数 `sort()`。

```cpp
list<int> L = {3, 1, 4, 1, 5, 9};

// sort() - 升序排序
L.sort();                          // L = {1, 1, 3, 4, 5, 9}

// 降序排序（自定义比较函数）
bool myCompare(int v1, int v2) {
    return v1 > v2;                 // 降序
}

L.sort(myCompare);                 // L = {9, 5, 4, 3, 1, 1}

// 使用 lambda 表达式
L.sort([](int a, int b) { return a > b; });
```

### 自定义类型排序

```cpp
struct Person {
    string name;
    int age;
};

list<Person> people = {
    {"Alice", 25},
    {"Bob", 30},
    {"Charlie", 20}
};

// 按年龄排序
people.sort([](const Person& a, const Person& b) {
    return a.age < b.age;
});

// 按姓名排序
people.sort([](const Person& a, const Person& b) {
    return a.name < b.name;
});
```

---

## 八、其他操作

### merge - 合并链表

```cpp
list<int> L1 = {1, 3, 5};
list<int> L2 = {2, 4, 6};

// 两个链表必须已排序
L1.merge(L2);                      // L1 = {1, 2, 3, 4, 5, 6}, L2 为空

// 自定义比较合并
L1.merge(L2, [](int a, int b) { return a > b; });  // 降序合并
```

### splice - 拼接链表

```cpp
list<int> L1 = {1, 2, 3};
list<int> L2 = {4, 5, 6};

auto it = L1.begin();
advance(it, 1);                    // L1: 1 -> it -> 2, 3

// 将 L2 的所有元素拼接到 L1 的 it 位置
L1.splice(it, L2);                 // L1 = {1, 4, 5, 6, 2, 3}

// 拼接单个元素
L1.splice(L1.begin(), L2, L2.begin());

// 拼接区间
L1.splice(L1.begin(), L2, L2.begin(), L2.end());
```

### unique - 去除连续重复元素

```cpp
list<int> L = {1, 1, 2, 2, 2, 3, 1};

L.unique();                        // L = {1, 2, 3, 1}
// 注意：只去除连续的重复，{1, 2, 1} 不会变化

// 先排序再 unique 可去除所有重复
L.sort();
L.unique();                        // L = {1, 2, 3}
```

---

## 九、链表的应用场景

### 1. 频繁的中间插入删除

```cpp
// 链表在已知位置插入删除是 O(1)，比 vector 高效
list<int> L = {1, 2, 4, 5};

// 找到插入位置
auto it = L.begin();
advance(it, 2);                     // 指向 4

// 插入 - O(1)
L.insert(it, 3);                    // L = {1, 2, 3, 4, 5}
```

### 2. 实现 LRU 缓存

```cpp
class LRUCache {
private:
    list<int> cacheList;             // 存储访问顺序
    unordered_map<int, list<int>::iterator> cacheMap;
    int capacity;

public:
    LRUCache(int cap) : capacity(cap) {}

    void get(int key) {
        if (cacheMap.find(key) != cacheMap.end()) {
            // 移到最前
            cacheList.push_front(key);
            cacheList.erase(cacheMap[key]);
            cacheMap[key] = cacheList.begin();
            return true;
        }
        return false;
    }

    void put(int key) {
        if (get(key)) return;       // 已存在，移到最前

        if (cacheList.size() >= capacity) {
            // 删除最久未使用的
            int last = cacheList.back();
            cacheList.pop_back();
            cacheMap.erase(last);
        }

        cacheList.push_front(key);
        cacheMap[key] = cacheList.begin();
    }
};
```

### 3. 实现撤销/重做

```cpp
class History {
private:
    list<string> states;
    list<string>::iterator current;

public:
    History() {
        states.push_back("");       // 初始状态
        current = states.begin();
    }

    void addState(const string& state) {
        // 删除当前位置之后的所有状态
        states.erase(next(current), states.end());
        states.push_back(state);
        current = prev(states.end());
    }

    string undo() {
        if (current != states.begin()) {
            --current;
        }
        return *current;
    }

    string redo() {
        if (current != prev(states.end())) {
            ++current;
        }
        return *current;
    }
};
```

---

## 十、注意事项

1. **不支持随机访问**：不能使用 `[]` 或 `at()`，只能通过迭代器访问
2. **迭代器移动**：链表不支持 `it + n`，需要使用 `advance(it, n)`
3. **排序使用成员函数**：必须使用 `L.sort()` 而非 `sort(L.begin(), L.end())`
4. **内存开销较大**：每个节点需要额外存储前驱和后继指针
5. **迭代器失效**：只有删除当前节点的迭代器会失效，插入不会失效

---

## 十一、常用函数速查表

| 分类 | 函数 | 功能 |
|------|------|------|
| 构造 | `list<T>()` | 默认构造 |
| | `list<T>(n, val)` | n 个 val |
| | `list<T>(begin, end)` | 迭代器区间构造 |
| 赋值 | `operator=` | 赋值 |
| | `assign()` | 赋值 |
| 大小 | `size()` | 元素个数 |
| | `empty()` | 是否为空 |
| | `resize()` | 调整大小 |
| 访问 | `front()`, `back()` | 首尾元素 |
| 头部操作 | `push_front()` | 头部插入 |
| | `pop_front()` | 头部删除 |
| 尾部操作 | `push_back()` | 尾部插入 |
| | `pop_back()` | 尾部删除 |
| 修改 | `insert()` | 插入 |
| | `erase()` | 删除 |
| | `remove()` | 删除指定值 |
| | `clear()` | 清空 |
| | `swap()` | 交换 |
| 操作 | `reverse()` | 反转 |
| | `sort()` | 排序 |
| | `merge()` | 合并 |
| | `splice()` | 拼接 |
| | `unique()` | 去重 |
| 迭代器 | `begin()`, `end()` | 正向迭代器 |
| | `rbegin()`, `rend()` | 反向迭代器 |

---

## 十二、力扣相关题目

| 题目 | 难度 | 考点 |
|------|------|------|
| [206. 反转链表](https://leetcode.cn/problems/reverse-linked-list/) | 简单 | 链表反转 |
| [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/) | 简单 | 链表合并 |
| [23. 合并 K 个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/) | 困难 | 链表合并 + 优先队列 |
| [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/) | 简单 | 快慢指针 |
| [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/) | 中等 | 快慢指针 |
| [146. LRU 缓存](https://leetcode.cn/problems/lru-cache/) | 中等 | 链表 + 哈希表 |
| [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/) | 中等 | 双指针 |
| [24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/) | 中等 | 链表操作 |
| [25. K 个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/) | 困难 | 链表反转 |
| [83. 删除排序链表中的重复元素](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/) | 简单 | 链表去重 |