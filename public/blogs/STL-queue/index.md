# C++ queue 学习笔记

> `#include <queue>` 是 C++ STL 中提供的队列容器。

---

## 什么是队列？

**队列** 是一种**先进先出**（FIFO，First In First Out）的线性数据结构。

### 队列的特点

```
入队方向 →
    ┌───┬───┬───┐
    │10 │20 │30 │
    └───┴───┴───┘
     ↑           ↑
  队头(front)  队尾(back)
     ← 出队方向
```

1. **先进先出**：最先插入的元素最先被取出
2. **双端受限**：只能在队尾插入、队头删除
3. **公平性**：先到先服务（如排队买票）

### 队列的基本操作

| 操作 | 说明 |
|------|------|
| 入队 `push()` | 在队尾插入元素 |
| 出队 `pop()` | 删除队头元素 |
| 查看队头 `front()` | 返回队头元素 |
| 查看队尾 `back()` | 返回队尾元素 |
| 判空 `empty()` | 判断队列是否为空 |
| 大小 `size()` | 返回队列中元素个数 |

---

## 一、构造函数

| 方式 | 代码示例 | 说明 |
|------|----------|------|
| 默认构造 | `queue<int> q;` | 创建空队列 |
| 拷贝构造 | `queue<int> q2(q1);` | 用另一个队列初始化 |

```cpp
#include <queue>
using namespace std;

// 默认构造
queue<int> q;                     // 创建空队列

// 拷贝构造
queue<intq> q1;
q1.push(1);
q1.push(2);
queue<int> q2(q1);                // 拷贝 q1
```

---

## 二、赋值操作

```cpp
queue<int> q1, q2;

q1.push(1);
q1.push(2);

// 赋值
q2 = q1;                          // q2 被赋值为 q1
```

---

## 三、数据存取

| 方法 | 代码示例 | 说明 |
|------|----------|------|
| `push()` | `q.push(elem);` | 在队尾插入元素 |
| `pop()` | `q.pop();` | 删除队头元素（不返回值） |
| `front()` | `int val = q.front();` | 返回队头元素 |
| `back()` | `int val = q.back();` | 返回队尾元素 |

```cpp
queue<int> q;

// push() - 入队
q.push(10);                       // 队列: [10]
q.push(20);                       // 队列: [10, 20]
q.push(30);                       // 队列: [10, 20, 30]

// front() - 查看队头
cout << q.front() << endl;       // 输出: 10

// back() - 查看队尾
cout << q.back() << endl;        // 输出: 30

// pop() - 出队
q.pop();                          // 弹出 10，队列: [20, 30]
cout << q.front() << endl;       // 输出: 20
```

---

## 四、大小操作

| 方法 | 说明 |
|------|------|
| `empty()` | 判断队列是否为空，返回 `true/false` |
| `size()` | 返回队列中元素个数 |

```cpp
queue<int> q;

cout << q.empty() << endl;       // 1 (true)
cout << q.size() << endl;         // 0

q.push(1);
q.push(2);
q.push(3);

cout << q.empty() << endl;       // 0 (false)
cout << q.size() << endl;         // 3
```

---

## 五、遍历队列

⚠️ **注意**：队列没有迭代器，需要通过弹出元素来遍历。

```cpp
queue<int> q;
q.push(1);
q.push(2);
q.push(3);

// 方式1：弹出遍历（会清空队列）
cout << "遍历队列: ";
while (!q.empty()) {
    cout << q.front() << " ";      // 输出: 1 2 3
    q.pop();
}
cout << endl;

// 方式2：使用临时队列（不破坏原队列）
queue<int> q2;
q2.push(1);
q2.push(2);
q2.push(3);

queue<int> temp = q2;              // 拷贝一份
while (!temp.empty()) {
    cout << temp.front() << " ";
    temp.pop();
}
```

---

## 六、队列的底层容器

`queue` 是一个**容器适配器**，默认使用 `deque` 作为底层容器。

```cpp
// 默认使用 deque
queue<int> q1;

// 指定底层容器为 list
queue<int, list<int>> q2;

// 注意：不能使用 vector（不支持 pop_front 操作）
```

---

## 七、队列的应用场景

### 1. BFS（广度优先搜索）

```cpp
#include <vector>
#include <queue>

vector<int> BFS(vector<vector<int>>& graph, int start) {
    int n = graph.size();
    vector<int> result;
    vector<bool> visited(n, false);
    queue<int> q;

    q.push(start);
    visited[start] = true;

    while (!q.empty()) {
        int node = q.front();
        q.pop();
        result.push_back(node);

        for (int neighbor : graph[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }

    return result;
}

// 图: 0 -> 1, 0 -> 2, 1 -> 3, 2 -> 3
// 从 0 开始 BFS: 0, 1, 2, 3
```

### 2. 层次遍历二叉树

```cpp
struct TreeNode {
    int val;
    TreeNode *left, *right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

vectorvector<int>> levelOrder(TreeNode* root) {
    vector<vector<int>> result;
    if (!root) return result;

    queue<TreeNode*> q;
    q.push(root);

    while (!q.empty()) {
        int levelSize = q.size();
        vector<int> level;

        for (int i = 0; i < levelSize; i++) {
            TreeNode* node = q.front();
            q.pop();
            level.push_back(node->val);

            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }

        result.push_back(level);
    }

    return result;
}

// 树:       1
//         /   \
//        2     3
//       / \
//      4   5
// 结果: [[1], [2,3], [4,5]]
```

### 3. 滑动窗口最大值（单调队列）

```cpp
#include <deque>

vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    vector<int> result;
    deque<int> q;  // 存储下标，保持递减顺序

    for (int i = 0; i < nums.size(); i++) {
        // 移除超出窗口的元素
        while (!q.empty() && q.front() <= i - k) {
            q.pop_front();
        }

        // 保持单调递减，移除比当前元素小的
        while (!q.empty() && nums[q.back()] < nums[i]) {
            q.pop_back();
        }

        q.push_back(i);

        // 窗口形成后，记录最大值
        if (i >= k - 1) {
            result.push_back(nums[q.front()]);
        }
    }

    return result;
}

// nums = [1,3,-1,-3,5,3,6,7], k = 3
// 结果: [3,3,5,5,6,7]
```

### 4. 任务调度

```cpp
// 按照请求时间顺序处理任务
class TaskScheduler {
private:
    queue<Task> taskQueue;

public:
    struct Task {
        int id;
        int priority;
        // 等其他属性...
    };

    void addTask(const Task& task) {
        taskQueue.push(task);
    }

    void processTasks() {
        while (!taskQueue.empty()) {
            Task current = taskQueue.front();
            taskQueue.pop();
            // 处理任务...
            cout << "Processing task " << current.id << endl;
        }
    }

    bool hasTasks() const {
        return !taskQueue.empty();
    }
};
```

### 5. 用队列实现栈

```cpp
class MyStack {
private:
    queue<int> q1, q2;
    bool useQ1 = true;

public:
    void push(int x) {
        // 将元素推入空队列
        if (useQ1) q2.push(x);
        else q1.push(x);

        // 将另一个队列的所有元素移过来
        queue<int>& src = useQ1 ? q1 : q2;
        queue<int>& dest = useQ1 ? q2 : q1;

        while (!src.empty()) {
            dest.push(src.front());
            src.pop();
        }

        useQ1 = !useQ1;
    }

    int pop() {
        int val = top();
        if (useQ1) q2.pop();
        else q1.pop();
        return val;
    }

    int top() {
        return useQ1 ? q2.front() : q1.front();
    }

    bool empty() {
        return useQ1 ? q2.empty() : q1.empty();
    }
};
```

---

## 八、优先队列（priority_queue）

`priority_queue` 是堆实现的优先队列，默认为大顶堆。

### 基本用法

```cpp
#include <queue>

// 默认大顶堆
priority_queue<int> pq1;
pq1.push(3);
pq1.push(1);
pq1.push(4);
pq1.push(1);

cout << pq1.top() << endl;       // 4

// 小顶堆
priority_queue<int, vector<int>, greater<int>> pq2;
pq2.push(3);
pq2.push(1);
pq2.push(4);

cout << pq2.top() << endl;       // 1

// 自定义比较
struct Node {
    int val;
    int priority;
    bool operator<(const Node& other) const {
        return priority < other.priority;  // 大顶堆
    }
};

priority_queue<Node> pq3;
pq3.push({1, 2});
pq3.push({2, 5});
pq3.push({3, 1});

cout << pq3.top().val << endl;   // 2 (priority=5 最大)
```

---

## 九、注意事项

1. **没有迭代器**：`queue` 不提供迭代器，不能直接遍历
2. **没有 clear()**：需要手动弹出所有元素来清空
3. **访问受限**：只能访问队头和队尾元素
4. **pop() 不返回值**：弹出操作不返回元素，需先用 `front()` 获取
5. **空队列操作**：对空队列调用 `pop()`、`front()` 或 `back()` 会导致未定义行为

---

## 十、常用函数速查表

### queue

| 分类 | 函数 | 功能 |
|------|------|------|
| 构造 | `queue<T>()` | 默认构造 |
| | `queue<T>(const queue&)` | 拷贝构造 |
| 赋值 | `operator=` | 赋值 |
| 大小 | `empty()` | 判空 |
| | `size()` | 元素个数 |
| 访问 | `front()` | 队头元素 |
| | `back()` | 队尾元素 |
| 修改 | `push()` | 入队 |
| | `pop()` | 出队 |

### priority_queue

| 分类 | 函数 | 功能 |
|------|------|------|
| 构造 | `priority_queue<T>()` | 默认大顶堆 |
| 大小 | `empty()` | 判空 |
| | `size()` | 元素个数 |
| 访问 | `top()` | 堆顶元素 |
| 修改 | `push()` | 插入元素 |
| | `pop()` | 删除堆顶 |

---

## 十一、力扣相关题目

| 题目 | 难度 | 考点 |
|------|------|------|
| [225. 用队列实现栈](https://leetcode.cn/problems/implement-stack-using-queues/) | 简单 | 双队列模拟栈 |
| [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/) | 简单 | 双栈模拟队列 |
| [346. 数据流中的移动平均值](https://leetcode.cn/problems/moving-average-from-data-stream/) | 简单 | 队列/滑动窗口 |
| [622. 设计循环队列](https://leetcode.cn/problems/design-circular-queue/) | 中等 | 循环队列 |
| [641. 设计循环双端队列](https://leetcode.cn/problems/design-circular-deque/) | 中等 | 循环双端队列 |
| [353. 数据流中的滑动窗口中位数](https://leetcode.cn/problems/sliding-window-median/) | 困难 | 双堆/单调队列 |
| [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/) | 困难 | 单调队列 |
| [862. 和至少为 K 的最短子数组](https://leetcode.cn/problems/shortest-subarray-with-sum-at-least-k/) | 困难 | 单调队列 |
| [502. IPO](https://leetcode.cn/problems/ipo/) | 困难 | 优先队列 + 贪心 |
| [621. 任务调度器](https://leetcode.cn/problems/task-scheduler/) | 中等 | 优先队列/贪心 |
