# C++ stack 学习笔记

> `#include <stack>` 是 C++ STL 中提供的栈容器。

---

## 什么是栈？

**栈** 是一种**先进后出**（LIFO，Last In First Out）的线性数据结构。

### 栈的特点

```
入栈方向 ↑
    ┌─────┐
    │ 30  │ ← 栈顶 (top)
    ├─────┤
    │ 20  │
    ├─────┤
    │ 10  │ ← 栈底 (bottom)
    └─────┘

出栈方向 ↓
```

1. **先进后出**：最后插入的元素最先被取出
2. **只能访问栈顶**：只能操作最顶端的元素
3. **受限的线性表**：插入和删除只能在一端（栈顶）进行

### 栈的基本操作

| 操作 | 说明 |
|------|------|
| 入栈 `push()` | 将元素压入栈顶 |
| 出栈 `pop()` | 弹出栈顶元素 |
| 查看栈顶 `top()` | 返回栈顶元素（不删除） |
| 判空 `empty()` | 判断栈是否为空 |
| 大小 `size()` | 返回栈中元素个数 |

---

## 一、构造函数

| 方式 | 代码示例 | 说明 |
|------|----------|------|
| 默认构造 | `stack<int> s;` | 创建空栈 |
| 拷贝构造 | `stack<int> s2(s1);` | 用另一个栈初始化 |

```cpp
#include <stack>
using namespace std;

// 默认构造
stack<int> s;                     // 创建空栈

// 拷贝构造
stack<int> s1;
s1.push(1);
s1.push(2);
stack<int> s2(s1);                // 拷贝 s1
```

---

## 二、赋值操作

```cpp
stack<int> s1, s2;

s1.push(1);
s1.push(2);

// 赋值
s2 = s1;                          // s2 被赋值为 s1
```

---

## 三、数据存取

| 方法 | 代码示例 | 说明 |
|------|----------|------|
| `push()` | `s.push(elem);` | 将元素压入栈顶 |
| `pop()` | `s.pop();` | 弹出栈顶元素（不返回值） |
| `top()` | `int val = s.top();` | 返回栈顶元素 |

```cpp
stack<int> s;

// push() - 入栈
s.push(10);                        // 栈: [10]
s.push(20);                        // 栈: [10, 20]
s.push(30);                        // 栈: [10, 20, 30]

// top() - 查看栈顶元素
cout << s.top() << endl;          // 输出: 30

// pop() - 出栈
s.pop();                           // 弹出 30，栈: [10, 20]
cout << s.top() << endl;          // 输出: 20
```

---

## 四、大小操作

| 方法 | 说明 |
|------|------|
| `empty()` | 判断栈是否为空，返回 `true/false` |
| `size()` | 返回栈中元素个数 |

```cpp
stack<int> s;

cout << s.empty() << endl;        // 1 (true)
cout << s.size() << endl;          // 0

s.push(1);
s.push(2);
s.push(3);

cout << s.empty() << endl;        // 0 (false)
cout << s.size() << endl;          // 3
```

---

## 五、遍历栈

⚠️ **注意**：栈没有提供迭代器，需要通过弹出元素来遍历。

```cpp
stack<int> s;
s.push(1);
s.push(2);
s.push(3);

// 方式1：弹出遍历（会清空栈）
cout << "遍历栈: ";
while (!s.empty()) {
    cout << s.top() << " ";       // 输出: 3 2 1
    s.pop();
}
cout << endl;

// 方式2：使用临时栈（不破坏原栈）
stack<int> s2;
s2.push(1);
s2.push(2);
s2.push(3);

stack<int> temp = s2;              // 拷贝一份
while (!temp.empty()) {
    cout << temp.top() << " ";
    temp.pop();
}
```

---

## 六、栈的底层容器

`stack` 是一个**容器适配器**，默认使用 `deque` 作为底层容器，也可以指定为 `vector` 或 `list`。

```cpp
// 默认使用 deque
stack<int> s1;

// 指定底层容器为 vector
stack<int, vector<int>> s2;

// 指定底层容器为 list
stack<int, list<int>> s3;
```

---

## 七、栈的应用场景

### 1. 括号匹配

```cpp
bool isValidParentheses(const string& s) {
    stack<char> st;

    for (char c : s) {
        if (c == '(' || c == '[' || c == '{') {
            st.push(c);
        } else {
            if (st.empty()) return false;

            char top = st.top();
            if ((c == ')' && top != '(') ||
                (c == ']' && top != '[') ||
                (c == '}' && top != '{')) {
                return false;
            }
            st.pop();
        }
    }

    return st.empty();
}

// 测试
cout << isValidParentheses("()[]{}") << endl;    // 1 (true)
cout << isValidParentheses("([)]") << endl;      // 0 (false)
```

### 2. 表达式求值（逆波兰表达式）

```cpp
int evalRPN(vector<string>& tokens) {
    stack<int> st;

    for (const string& token : tokens) {
        if (token == "+" || token == "-" || token == "*" || token == "/") {
            int b = st.top(); st.pop();
            int a = st.top(); st.pop();
            int result;
            if (token == "+") result = a + b;
            else if (token == "-") result = a - b;
            else if (token == "*") result = a * b;
            else if (token == "/") result = a / b;
            st.push(result);
        } else {
            st.push(stoi(token));
        }
    }

    return st.top();
}

// 测试：["2","1","+","3","*"] = (2+1)*3 = 9
```

### 3. 进制转换

```cpp
string decimalToBinary(int n) {
    if (n == 0) return "0";

    stack<int> st;
    while (n > 0) {
        st.push(n % 2);
        n /= 2;
    }

    string result;
    while (!st.empty()) {
        result += to_string(st.top());
        st.pop();
    }
    return result;
}

// 测试
cout << decimalToBinary(10) << endl;    // 输出: 1010
```

### 4. 函数调用栈（递归）

```cpp
// 递归实现的阶乘（隐式使用栈）
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}

// 等价的非递归实现（显式使用栈）
int factorialIterative(int n) {
    stack<int> st;
    int result = 1;

    for (int i = 2; i <= n; i++) {
        st.push(i);
    }

    while (!st.empty()) {
        result *= st.top();
        st.pop();
    }
    return result;
}
```

### 5. 撤销操作（Undo）

```cpp
class TextEditor {
private:
    stack<string> history;
    stack<string> future;
    string current;

public:
    void write(const string& text) {
        history.push(current);
        current = text;
        while (!future.empty()) future.pop();  // 清空 redo 栈
    }

    void undo() {
        if (!history.empty()) {
            future.push(current);
            current = history.top();
            history.pop();
        }
    }

    void redo() {
        if (!future.empty()) {
            history.push(current);
            current = future.top();
            future.pop();
        }
    }

    string getText() { return current; }
};
```

---

## 八、单调栈

单调栈是栈的一个重要应用，用于解决"下一个更大/更小元素"类问题的问题。

### 寻找下一个更大元素

```cpp
vector<int> nextGreaterElements(vector<int>& nums) {
    int n = nums.size();
    vector<int> result(n, -1);
    stack<int> st;  // 存储下标

    for (int i = 0; i < n; i++) {
        while (!st.empty() && nums[st.top()] < nums[i]) {
            result[st.top()] = nums[i];
            st.pop();
        }
        st.push(i);
    }

    return result;
}

// 测试: [2, 1, 2, 4, 3] -> [4, 2, 4, -1, -1]
// 2 的下一个更大元素是 4
// 1 的下一个更大元素是 2
```

---

## 九、注意事项

1. **没有迭代器**：`stack` 不提供迭代器，不能直接遍历
2. **没有 clear()**：需要手动弹出所有元素来清空
3. **访问受限**：只能访问栈顶元素，不能访问中间或底部元素
4. **pop() 不返回值**：弹出操作不返回元素，需先用 `top()` 获取
5. **空栈操作**：对空栈调用 `pop()` 或 `top()` 会导致未定义行为

---

## 十、常用函数速查表

| 分类 | 函数 | 功能 |
|------|------|------|
| 构造 | `stack<T>()` | 默认构造 |
| | `stack<T>(const stack&)` | 拷贝构造 |
| 赋值 | `operator=` | 赋值 |
| 大小 | `empty()` | 判空 |
| | `size()` | 元素个数 |
| 访问 | `top()` | 栈顶元素 |
| 修改 | `push()` | 入栈 |
| | `pop()` | 出栈 |

---

## 十一、力扣相关题目

| 题目 | 难度 | 考点 |
|------|------|------|
| [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/) | 简单 | 括号匹配 |
| [155. 最小栈](https://leetcode.cn/problems/min-stack/) | 中等 | 辅助栈 |
| [232. 用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/) | 简单 | 双栈模拟队列 |
| [496. 下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/) | 简单 | 单调栈 |
| [739. 每日温度](https://leetcode.cn/problems/daily-temperatures/) | 中等 | 单调栈 |
| [150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/) | 中等 | 栈模拟计算 |
| [856. 括号的分数](https://leetcode.cn/problems/score-of-parentheses/) | 中等 | 栈 |
| [901. 股票价格跨度](https://leetcode.cn/problems/online-stock-span/) | 中等 | 单调栈 |
| [1021. 删除最外层的括号](https://leetcode.cn/problems/remove-outermost-parentheses/) | 简单 | 栈 |
| [1047. 删除字符串中的所有相邻重复项](https://leetcode.cn/problems/remove-all-adjacent-duplicates-in-string/) | 简单 | 栈 |
