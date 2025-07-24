#  C++ 标准库实用数据类型总览

---

##  一、容器（Containers）

| 类型                                  | 头文件            | 简述                      |
| ------------------------------------- | ----------------- | ------------------------- |
| `vector`                              | `<vector>`        | 动态数组，支持随机访问    |
| `deque`                               | `<deque>`         | 双端队列，支持头尾操作    |
| `list`                                | `<list>`          | 双向链表                  |
| `forward_list`                        | `<forward_list>`  | 单向链表（C++11）         |
| `array`                               | `<array>`         | 静态数组（C++11）         |
| `set`, `multiset`                     | `<set>`           | 有序集合（基于红黑树）    |
| `map`, `multimap`                     | `<map>`           | 有序映射                  |
| `unordered_set`, `unordered_multiset` | `<unordered_set>` | 无序集合（基于哈希表）    |
| `unordered_map`, `unordered_multimap` | `<unordered_map>` | 无序映射                  |
| `stack`                               | `<stack>`         | 栈，容器适配器            |
| `queue`                               | `<queue>`         | 队列，容器适配器          |
| `priority_queue`                      | `<queue>`         | 优先队列（堆）            |
| `bitset`                              | `<bitset>`        | 位数组                    |
| `valarray`                            | `<valarray>`      | 面向数值计算的数组        |
| `span`                                | `<span>`          | 非拥有的数组视图（C++20） |

---

##  二、元组与结构组合类型

| 类型                 | 头文件      | 简述                         |
| -------------------- | ----------- | ---------------------------- |
| `pair`               | `<utility>` | 二元组                       |
| `tuple`              | `<tuple>`   | 多元组，任意类型组合         |
| `structured_binding` | C++17       | `auto [x, y] = ...` 解构语法 |

---

##  三、值封装与类型包装器

| 类型             | 头文件       | 简述                  |
| ---------------- | ------------ | --------------------- |
| `optional<T>`    | `<optional>` | 可选值（C++17）       |
| `variant<Ts...>` | `<variant>`  | 安全联合体（C++17）   |
| `any`            | `<any>`      | 类型擦除容器（C++17） |
| `expected<T, E>` | `<expected>` | 错误处理类型（C++23） |

---

##  四、智能指针

| 类型                         | 头文件     | 简述                 |
| ---------------------------- | ---------- | -------------------- |
| `shared_ptr`                 | `<memory>` | 引用计数共享指针     |
| `unique_ptr`                 | `<memory>` | 独占所有权指针       |
| `weak_ptr`                   | `<memory>` | 弱引用指针           |
| `make_shared`, `make_unique` | `<memory>` | 安全创建智能指针实例 |

---

##  五、字符串类型

| 类型                     | 头文件          | 简述                            |
| ------------------------ | --------------- | ------------------------------- |
| `string`                 | `<string>`      | 动态字符串                      |
| `wstring`, `u8string` 等 | `<string>`      | 宽字符/Unicode 字符串           |
| `string_view`            | `<string_view>` | 字符串视图，不复制数据（C++17） |

---

##  六、时间与日期

| 类型                           | 头文件     | 简述                  |
| ------------------------------ | ---------- | --------------------- |
| `duration`, `time_point`       | `<chrono>` | 时间间隔与时间点      |
| `system_clock`, `steady_clock` | `<chrono>` | 系统时钟/单调时钟     |
| `year`, `month`, `zoned_time`  | `<chrono>` | 高级日期时间（C++20） |

---

##  七、函数封装与适配器

| 类型                            | 头文件         | 简述                    |
| ------------------------------- | -------------- | ----------------------- |
| `function`                      | `<functional>` | 可调用对象包装器        |
| `bind`, `mem_fn`, `ref`, `cref` | `<functional>` | 函数适配与引用包装      |
| `invoke`                        | `<functional>` | 通用函数调用器（C++17） |

---

##  八、并发相关类型

| 类型                                 | 头文件                 | 简述                |
| ------------------------------------ | ---------------------- | ------------------- |
| `thread`                             | `<thread>`             | 线程类              |
| `mutex`, `lock_guard`, `unique_lock` | `<mutex>`              | 互斥锁工具          |
| `condition_variable`                 | `<condition_variable>` | 条件变量            |
| `future`, `promise`, `async`         | `<future>`             | 异步结果与任务      |
| `atomic<T>`                          | `<atomic>`             | 原子变量            |
| `jthread`, `stop_token`              | `<thread>`             | 可中止线程（C++20） |

---

##  九、错误处理类型

| 类型                            | 头文件           | 简述                            |
| ------------------------------- | ---------------- | ------------------------------- |
| `exception`, `runtime_error` 等 | `<stdexcept>`    | 标准异常体系                    |
| `error_code`, `error_condition` | `<system_error>` | 错误码封装                      |
| `expected`                      | `<expected>`     | 代替异常的新式错误处理（C++23） |

---

##  十、通用与工具类型

| 类型                                     | 头文件               | 简述                       |
| ---------------------------------------- | -------------------- | -------------------------- |
| `initializer_list`                       | `<initializer_list>` | 支持花括号初始化           |
| `type_traits`                            | `<type_traits>`      | 元编程与类型萃取工具       |
| `regex`                                  | `<regex>`            | 正则表达式引擎             |
| `filesystem::path`, `directory_iterator` | `<filesystem>`       | 文件/目录路径处理（C++17） |
| `random`                                 | `<random>`           | 随机数生成与分布模型       |
| `numeric_limits`                         | `<limits>`           | 获取类型范围               |
| `bit_*` 函数                             | `<bit>`              | 位操作函数（C++20）        |
| `source_location`                        | `<source_location>`  | 当前代码位置（C++20）      |

---

# C++ 标准库实用数据类型函数总览

## 一、容器（Containers）

### 1. `vector`
- **构造与初始化**
  - `vector()`: 默认构造函数
  - `vector(size_type n)`: 创建包含n个元素的向量
  - `vector(initializer_list<T> init)`: 用初始化列表构造
- **元素访问**
  - `at(size_type pos)`: 访问指定位置元素，带边界检查
  - `operator[]`: 访问指定位置元素
  - `front()`: 返回第一个元素
  - `back()`: 返回最后一个元素
  - `data()`: 返回指向内存数组的指针
- **容量**
  - `empty()`: 检查容器是否为空
  - `size()`: 返回当前元素数量
  - `capacity()`: 返回当前容量
  - `reserve(size_type new_cap)`: 预留存储空间
  - `shrink_to_fit()`: 请求减小容量以匹配大小
- **修改**
  - `push_back(const T& value)`: 在尾部添加元素
  - `pop_back()`: 移除尾部元素
  - `insert(iterator pos, const T& value)`: 在指定位置插入元素
  - `erase(iterator pos)`: 移除指定位置元素
  - `clear()`: 清空容器
  - `resize(size_type count)`: 调整容器大小

### 2. `deque`
- **构造与初始化**
  - `deque()`: 默认构造函数
  - `deque(size_type n)`: 创建包含n个元素的双端队列
- **元素访问**
  - `at(size_type pos)`: 访问指定位置元素
  - `operator[]`: 访问指定位置元素
  - `front()`: 返回第一个元素
  - `back()`: 返回最后一个元素
- **容量**
  - `empty()`: 检查容器是否为空
  - `size()`: 返回元素数量
- **修改**
  - `push_back(const T& value)`: 在尾部添加元素
  - `push_front(const T& value)`: 在头部添加元素
  - `pop_back()`: 移除尾部元素
  - `pop_front()`: 移除头部元素
  - `insert(iterator pos, const T& value)`: 在指定位置插入元素
  - `erase(iterator pos)`: 移除指定位置元素

### 3. `list`
- **构造与初始化**
  - `list()`: 默认构造函数
  - `list(size_type n)`: 创建包含n个元素的列表
- **元素访问**
  - `front()`: 返回第一个元素
  - `back()`: 返回最后一个元素
- **容量**
  - `empty()`: 检查容器是否为空
  - `size()`: 返回元素数量
- **修改**
  - `push_back(const T& value)`: 在尾部添加元素
  - `push_front(const T& value)`: 在头部添加元素
  - `pop_back()`: 移除尾部元素
  - `pop_front()`: 移除头部元素
  - `insert(iterator pos, const T& value)`: 在指定位置插入元素
  - `erase(iterator pos)`: 移除指定位置元素
  - `sort()`: 对元素进行排序
  - `merge(list& other)`: 合并两个已排序列表
  - `splice(iterator pos, list& other)`: 转移元素

### 4. `forward_list`
- **构造与初始化**
  - `forward_list()`: 默认构造函数
  - `forward_list(size_type n)`: 创建包含n个元素的单向链表
- **元素访问**
  - `front()`: 返回第一个元素
- **容量**
  - `empty()`: 检查容器是否为空
  - `max_size()`: 返回最大可能元素数量
- **修改**
  - `push_front(const T& value)`: 在头部添加元素
  - `pop_front()`: 移除头部元素
  - `insert_after(iterator pos, const T& value)`: 在指定位置后插入元素
  - `erase_after(iterator pos)`: 移除指定位置后的元素
  - `sort()`: 对元素进行排序
  - `merge(forward_list& other)`: 合并两个已排序链表

### 5. `array`
- **构造与初始化**
  - `array()`: 默认构造函数（元素值未定义）
  - 使用聚合初始化: `array<int, 3> a = {1, 2, 3};`
- **元素访问**
  - `at(size_type pos)`: 访问指定位置元素，带边界检查
  - `operator[]`: 访问指定位置元素
  - `front()`: 返回第一个元素
  - `back()`: 返回最后一个元素
  - `data()`: 返回指向内存数组的指针
- **容量**
  - `empty()`: 检查容器是否为空
  - `size()`: 返回元素数量（固定值）
- **操作**
  - `fill(const T& value)`: 用指定值填充所有元素
  - `swap(array& other)`: 交换两个数组内容

### 6. `set`/`multiset`
- **构造与初始化**
  - `set()`: 默认构造函数
  - `set(initializer_list<T> init)`: 用初始化列表构造
- **元素访问**
  - `find(const Key& key)`: 查找元素
  - `count(const Key& key)`: 返回匹配元素数量
  - `lower_bound(const Key& key)`: 返回首个不小于key的元素迭代器
  - `upper_bound(const Key& key)`: 返回首个大于key的元素迭代器
- **容量**
  - `empty()`: 检查容器是否为空
  - `size()`: 返回元素数量
- **修改**
  - `insert(const value_type& value)`: 插入元素
  - `erase(const Key& key)`: 移除指定键的元素
  - `clear()`: 清空容器

### 7. `map`/`multimap`
- **构造与初始化**
  - `map()`: 默认构造函数
  - `map(initializer_list<value_type> init)`: 用初始化列表构造
- **元素访问**
  - `at(const Key& key)`: 访问指定键的元素，带边界检查
  - `operator[](const Key& key)`: 访问或插入指定键的元素
  - `find(const Key& key)`: 查找元素
  - `count(const Key& key)`: 返回匹配键的元素数量
- **容量**
  - `empty()`: 检查容器是否为空
  - `size()`: 返回元素数量
- **修改**
  - `insert(const value_type& value)`: 插入元素
  - `erase(const Key& key)`: 移除指定键的元素
  - `clear()`: 清空容器

### 8. `unordered_set`/`unordered_multiset`
- **构造与初始化**
  - `unordered_set()`: 默认构造函数
  - `unordered_set(size_type bucket_count)`: 指定桶数量构造
- **元素访问**
  - `find(const Key& key)`: 查找元素
  - `count(const Key& key)`: 返回匹配元素数量
- **容量**
  - `empty()`: 检查容器是否为空
  - `size()`: 返回元素数量
- **修改**
  - `insert(const value_type& value)`: 插入元素
  - `erase(const Key& key)`: 移除指定键的元素
  - `clear()`: 清空容器
- **哈希策略**
  - `rehash(size_type n)`: 设置桶数量并重新哈希
  - `reserve(size_type count)`: 预留至少能容纳count个元素的空间

### 9. `unordered_map`/`unordered_multimap`
- **构造与初始化**
  - `unordered_map()`: 默认构造函数
  - `unordered_map(size_type bucket_count)`: 指定桶数量构造
- **元素访问**
  - `at(const Key& key)`: 访问指定键的元素，带边界检查
  - `operator[](const Key& key)`: 访问或插入指定键的元素
  - `find(const Key& key)`: 查找元素
  - `count(const Key& key)`: 返回匹配键的元素数量
- **容量**
  - `empty()`: 检查容器是否为空
  - `size()`: 返回元素数量
- **修改**
  - `insert(const value_type& value)`: 插入元素
  - `erase(const Key& key)`: 移除指定键的元素
  - `clear()`: 清空容器
- **哈希策略**
  - `rehash(size_type n)`: 设置桶数量并重新哈希
  - `reserve(size_type count)`: 预留至少能容纳count个元素的空间

### 10. `stack`
- **元素访问**
  - `top()`: 返回栈顶元素
- **容量**
  - `empty()`: 检查栈是否为空
  - `size()`: 返回栈中元素数量
- **修改**
  - `push(const value_type& value)`: 压入元素
  - `pop()`: 弹出栈顶元素

### 11. `queue`
- **元素访问**
  - `front()`: 返回队首元素
  - `back()`: 返回队尾元素
- **容量**
  - `empty()`: 检查队列是否为空
  - `size()`: 返回队列中元素数量
- **修改**
  - `push(const value_type& value)`: 入队
  - `pop()`: 出队

### 12. `priority_queue`
- **元素访问**
  - `top()`: 返回优先级最高的元素
- **容量**
  - `empty()`: 检查队列是否为空
  - `size()`: 返回队列中元素数量
- **修改**
  - `push(const value_type& value)`: 插入元素
  - `pop()`: 移除优先级最高的元素

### 13. `bitset`
- **构造与初始化**
  - `bitset()`: 默认构造函数，所有位初始化为0
  - `bitset(unsigned long long val)`: 用整数值初始化
  - `bitset(const string& str)`: 用字符串初始化
- **元素访问**
  - `operator[](size_t pos)`: 访问指定位置的位
  - `test(size_t pos)`: 检查指定位置的位是否为1
  - `all()`: 检查所有位是否为1
  - `any()`: 检查是否有任何位为1
  - `none()`: 检查所有位是否为0
- **修改**
  - `set()`: 设置所有位为1
  - `set(size_t pos, bool value = true)`: 设置指定位置的位
  - `reset()`: 重置所有位为0
  - `reset(size_t pos)`: 重置指定位置的位
  - `flip()`: 翻转所有位
  - `flip(size_t pos)`: 翻转指定位置的位
- **操作**
  - `to_ulong()`: 转换为unsigned long类型
  - `to_ullong()`: 转换为unsigned long long类型
  - `to_string()`: 转换为字符串

### 14. `valarray`
- **构造与初始化**
  - `valarray()`: 默认构造函数
  - `valarray(size_t n)`: 创建包含n个元素的valarray
  - `valarray(const T& value, size_t n)`: 用value初始化n个元素
- **元素访问**
  - `operator[]`: 访问指定位置的元素
- **容量**
  - `size()`: 返回元素数量
- **操作**
  - `sum()`: 计算所有元素的和
  - `min()`: 返回最小值
  - `max()`: 返回最大值
  - `apply(T func(const T&))`: 对每个元素应用函数
  - `shift(int n)`: 循环移位
  - `cshift(int n)`: 循环旋转

### 15. `span`
- **构造与初始化**
  - `span()`: 默认构造函数
  - `span(T* data, size_t size)`: 从指针和大小构造
  - `span(Container& cont)`: 从容器构造
- **元素访问**
  - `operator[]`: 访问指定位置的元素
  - `front()`: 返回第一个元素
  - `back()`: 返回最后一个元素
  - `data()`: 返回指向数据的指针
- **容量**
  - `empty()`: 检查是否为空
  - `size()`: 返回元素数量
  - `size_bytes()`: 返回数据大小（字节）
- **子视图**
  - `first(size_t count)`: 返回前count个元素的子span
  - `last(size_t count)`: 返回后count个元素的子span
  - `subspan(size_t offset, size_t count = dynamic_extent)`: 返回子span

## 二、元组与结构组合类型

### 1. `pair`
- **构造与初始化**
  - `pair()`: 默认构造函数
  - `pair(const T1& x, const T2& y)`: 用两个值构造
  - `make_pair(T1 x, T2 y)`: 辅助函数创建pair
- **元素访问**
  - `first`: 访问第一个元素
  - `second`: 访问第二个元素
- **操作**
  - `swap(pair& other)`: 交换两个pair的内容

### 2. `tuple`
- **构造与初始化**
  - `tuple()`: 默认构造函数
  - `tuple(const Types&... args)`: 用参数包构造
  - `make_tuple(const Types&... args)`: 辅助函数创建tuple
- **元素访问**
  - `get<I>(tuple)`: 获取第I个元素
  - `tuple_size<tuple_type>::value`: 获取元素数量
  - `tuple_element<I, tuple_type>::type`: 获取第I个元素的类型
- **操作**
  - `tie(elements...)`: 解包tuple到引用
  - `tuple_cat(tuples...)`: 连接多个tuple

### 3. `structured_binding`
- **解构语法**
  - `auto [a, b] = pair_or_tuple`: 解构pair或tuple
  - `auto [a, b, c] = struct_or_array`: 解构结构体或数组

## 三、值封装与类型包装器

### 1. `optional<T>`
- **构造与初始化**
  - `optional()`: 创建空optional
  - `optional(const T& value)`: 用值初始化
  - `make_optional(value)`: 辅助函数创建optional
- **状态检查**
  - `has_value()`: 检查是否包含值
  - `operator bool()`: 检查是否包含值
- **值访问**
  - `value()`: 获取值，若为空则抛出异常
  - `value_or(default_value)`: 获取值，若为空则返回默认值
  - `operator*()`: 解引用值
- **修改**
  - `reset()`: 清空optional
  - `emplace(args...)`: 就地构造值

### 2. `variant<Ts...>`
- **构造与初始化**
  - `variant()`: 默认构造第一个类型的默认值
  - `variant(const T& value)`: 用特定类型的值初始化
- **状态检查**
  - `index()`: 返回当前活跃类型的索引
  - `holds_alternative<T>()`: 检查是否持有特定类型的值
- **值访问**
  - `get<I>(variant)`: 通过索引获取值
  - `get<T>(variant)`: 通过类型获取值
  - `get_if<I>(&variant)`: 通过索引安全获取指针
  - `get_if<T>(&variant)`: 通过类型安全获取指针
- **操作**
  - `visit(visitor, variants...)`: 对variant应用访问者模式

### 3. `any`
- **构造与初始化**
  - `any()`: 创建空any
  - `any(const T& value)`: 用值初始化
- **状态检查**
  - `has_value()`: 检查是否包含值
  - `type()`: 返回存储值的类型信息
- **值访问**
  - `any_cast<T>(any)`: 转换为特定类型
  - `any_cast<T*>(&any)`: 安全转换为指针
- **修改**
  - `reset()`: 清空any
  - `emplace<T>(args...)`: 就地构造值

### 4. `expected<T, E>`
- **构造与初始化**
  - `expected(T value)`: 用成功值初始化
  - `unexpected(E error)`: 用错误值初始化
- **状态检查**
  - `has_value()`: 检查是否包含成功值
- **值访问**
  - `value()`: 获取成功值，若失败则抛出异常
  - `error()`: 获取错误值
- **操作**
  - `and_then(func)`: 链式处理成功值
  - `or_else(func)`: 链式处理错误值

## 四、智能指针

### 1. `shared_ptr`
- **构造与初始化**
  - `shared_ptr()`: 创建空shared_ptr
  - `shared_ptr(T* ptr)`: 从原始指针构造
  - `make_shared(args...)`: 安全创建shared_ptr
- **引用计数**
  - `use_count()`: 返回引用计数
  - `unique()`: 检查是否唯一拥有对象
- **操作**
  - `reset()`: 释放所有权
  - `reset(T* ptr)`: 重置为指向新对象
  - `swap(shared_ptr& other)`: 交换内容
- **访问**
  - `operator*()`: 解引用
  - `operator->()`: 访问成员
  - `get()`: 返回原始指针

### 2. `unique_ptr`
- **构造与初始化**
  - `unique_ptr()`: 创建空unique_ptr
  - `unique_ptr(T* ptr)`: 从原始指针构造
  - `make_unique(args...)`: 安全创建unique_ptr
- **操作**
  - `reset()`: 释放所有权
  - `reset(T* ptr)`: 重置为指向新对象
  - `swap(unique_ptr& other)`: 交换内容
  - `release()`: 释放所有权并返回原始指针
- **访问**
  - `operator*()`: 解引用
  - `operator->()`: 访问成员
  - `get()`: 返回原始指针

### 3. `weak_ptr`
- **构造与初始化**
  - `weak_ptr()`: 创建空weak_ptr
  - `weak_ptr(const shared_ptr<T>& other)`: 从shared_ptr构造
- **操作**
  - `expired()`: 检查引用的对象是否已被删除
  - `lock()`: 创建shared_ptr共享对象所有权
  - `reset()`: 释放引用
  - `swap(weak_ptr& other)`: 交换内容
- **引用计数**
  - `use_count()`: 返回引用计数

### 4. `make_shared`, `make_unique`
- `make_shared(args...)`: 创建shared_ptr的工厂函数
- `make_unique(args...)`: 创建unique_ptr的工厂函数（C++14起）

## 五、字符串类型

### 1. `string`
- **构造与初始化**
  - `string()`: 默认构造函数
  - `string(const char* s)`: 从C风格字符串构造
  - `string(const string& str)`: 拷贝构造函数
  - `string(initializer_list<char> il)`: 用初始化列表构造
- **元素访问**
  - `operator[]`: 访问指定位置的字符
  - `at(size_type pos)`: 访问指定位置的字符，带边界检查
  - `front()`: 返回第一个字符
  - `back()`: 返回最后一个字符
- **容量**
  - `empty()`: 检查字符串是否为空
  - `size()`: 返回字符串长度
  - `capacity()`: 返回当前容量
  - `reserve(size_type new_cap)`: 预留存储空间
  - `shrink_to_fit()`: 请求减小容量以匹配大小
- **修改**
  - `push_back(char c)`: 在尾部添加字符
  - `pop_back()`: 移除尾部字符
  - `append(const string& str)`: 追加字符串
  - `operator+=(const string& str)`: 追加字符串
  - `insert(size_type pos, const string& str)`: 在指定位置插入字符串
  - `erase(size_type pos = 0, size_type count = npos)`: 移除子字符串
  - `replace(size_type pos, size_type count, const string& str)`: 替换子字符串
- **操作**
  - `c_str()`: 返回C风格字符串
  - `substr(size_type pos = 0, size_type count = npos)`: 返回子字符串
  - `find(const string& str, size_type pos = 0)`: 查找子字符串
  - `rfind(const string& str, size_type pos = npos)`: 反向查找子字符串
  - `compare(const string& str)`: 比较字符串

### 2. `wstring`, `u8string`, `u16string`, `u32string`
- 与`string`类似，但分别用于宽字符、UTF-8、UTF-16和UTF-32编码

### 3. `string_view`
- **构造与初始化**
  - `string_view()`: 默认构造函数
  - `string_view(const char* str)`: 从C风格字符串构造
  - `string_view(const string& str)`: 从string构造
- **元素访问**
  - `operator[]`: 访问指定位置的字符
  - `at(size_type pos)`: 访问指定位置的字符，带边界检查
  - `front()`: 返回第一个字符
  - `back()`: 返回最后一个字符
  - `data()`: 返回指向字符数组的指针
- **容量**
  - `empty()`: 检查是否为空
  - `size()`: 返回长度
- **子视图**
  - `substr(size_type pos = 0, size_type count = npos)`: 返回子视图
  - `remove_prefix(size_type n)`: 移除前n个字符
  - `remove_suffix(size_type n)`: 移除后n个字符
- **查找**
  - `find(const string_view& str, size_type pos = 0)`: 查找子视图
  - `rfind(const string_view& str, size_type pos = npos)`: 反向查找子视图

## 六、时间与日期

### 1. `duration`
- **构造与初始化**
  - `duration()`: 默认构造函数
  - `duration(Rep value)`: 用计数值构造
- **操作**
  - `count()`: 返回计数值
  - `zero()`: 返回表示零时间的duration
  - `min()`: 返回最小可能的duration
  - `max()`: 返回最大可能的duration
- **算术运算**
  - `operator+`, `operator-`, `operator*`, `operator/`, `operator%`
  - `operator++`, `operator--`

### 2. `time_point`
- **构造与初始化**
  - `time_point()`: 默认构造函数（表示时钟的纪元）
  - `time_point(duration d)`: 从duration构造
- **操作**
  - `time_since_epoch()`: 返回从纪元开始的duration
  - `min()`: 返回最小可能的time_point
  - `max()`: 返回最大可能的time_point
- **算术运算**
  - `operator+`, `operator-`
  - `operator==`, `operator!=`, `operator<`, `operator<=`, `operator>`, `operator>=`

### 3. `system_clock`, `steady_clock`, `high_resolution_clock`
- **公共函数**
  - `now()`: 返回当前时间点
  - `to_time_t(const time_point& t)`: 转换为time_t类型
  - `from_time_t(time_t t)`: 从time_t类型转换

### 4. C++20 日期时间库
- **日期类型**
  - `year`, `month`, `day`, `year_month`, `year_month_day`
- **时间点类型**
  - `zoned_time`, `local_time`
- **格式化与解析**
  - `format(format_str, tp)`: 格式化时间点
  - `parse(format_str, str)`: 解析字符串为时间点

## 七、函数封装与适配器

### 1. `function`
- **构造与初始化**
  - `function()`: 默认构造函数
  - `function(Callable&& f)`: 从可调用对象构造
- **状态检查**
  - `operator bool()`: 检查是否包含可调用对象
- **调用**
  - `operator()`: 调用存储的可调用对象

### 2. `bind`, `mem_fn`, `ref`, `cref`
- `bind(func, args...)`: 绑定函数和参数
- `mem_fn(&Class::member)`: 创建成员函数适配器
- `ref(obj)`: 创建引用包装器
- `cref(obj)`: 创建常量引用包装器

### 3. `invoke`
- `invoke(func, args...)`: 通用函数调用
- 支持普通函数、成员函数、成员变量等

## 八、并发相关类型

### 1. `thread`
- **构造与初始化**
  - `thread()`: 默认构造函数
  - `thread(Callable&& f, Args&&... args)`: 用可调用对象和参数构造
- **操作**
  - `join()`: 等待线程完成
  - `detach()`: 分离线程
  - `joinable()`: 检查线程是否可连接
  - `get_id()`: 获取线程ID
  - `hardware_concurrency()`: 返回硬件支持的并发线程数

### 2. `mutex`, `lock_guard`, `unique_lock`
- **`mutex`**
  - `lock()`: 锁定互斥量
  - `try_lock()`: 尝试锁定互斥量
  - `unlock()`: 解锁互斥量
- **`lock_guard`**
  - `lock_guard(mutex_type& m)`: 构造时锁定，析构时解锁
- **`unique_lock`**
  - 更灵活的锁管理，支持延迟锁定、超时等

### 3. `condition_variable`
- **操作**
  - `wait(unique_lock<mutex>& lock)`: 等待条件
  - `wait_for(unique_lock<mutex>& lock, duration d)`: 等待条件或超时
  - `wait_until(unique_lock<mutex>& lock, time_point tp)`: 等待条件直到时间点
  - `notify_one()`: 唤醒一个等待线程
  - `notify_all()`: 唤醒所有等待线程

### 4. `future`, `promise`, `async`
- **`future`**
  - `get()`: 获取异步结果
  - `wait()`: 等待结果就绪
  - `wait_for(duration d)`: 等待结果或超时
  - `wait_until(time_point tp)`: 等待结果直到时间点
- **`promise`**
  - `set_value(value)`: 设置结果值
  - `set_exception(exception_ptr p)`: 设置异常
  - `get_future()`: 获取关联的future
- **`async`**
  - `async(launch policy, Callable&& f, Args&&... args)`: 异步执行函数

### 5. `atomic<T>`
- **操作**
  - `store(T desired)`: 原子存储值
  - `load()`: 原子加载值
  - `exchange(T desired)`: 原子交换值
  - `compare_exchange_weak(expected, desired)`: 弱比较交换
  - `compare_exchange_strong(expected, desired)`: 强比较交换
- **算术操作**
  - `fetch_add`, `fetch_sub`, `fetch_and`, `fetch_or`, `fetch_xor`

### 6. `jthread`, `stop_token`
- **`jthread`**
  - 自动管理线程生命周期，支持协作式取消
- **`stop_token`**
  - 检查是否请求停止
- **`stop_source`**
  - 发出停止请求

## 九、错误处理类型

### 1. 标准异常类
- **基类**
  - `exception`: 所有标准异常的基类
- **派生类**
  - `logic_error`: 逻辑错误基类
    - `domain_error`, `invalid_argument`, `length_error`, `out_of_range`
  - `runtime_error`: 运行时错误基类
    - `range_error`, `overflow_error`, `underflow_error`
- **操作**
  - `what()`: 返回错误描述信息

### 2. `error_code`, `error_condition`
- **`error_code`**
  - 表示特定平台的错误码
- **`error_condition`**
  - 表示可移植的错误条件
- **操作**
  - `value()`: 返回错误码值
  - `category()`: 返回错误类别
  - `message()`: 返回错误消息

### 3. `expected`
- **构造与初始化**
  - `expected(T value)`: 用成功值初始化
  - `unexpected(E error)`: 用错误值初始化
- **状态检查**
  - `has_value()`: 检查是否包含成功值
- **值访问**
  - `value()`: 获取成功值，若失败则抛出异常
  - `error()`: 获取错误值
- **操作**
  - `and_then(func)`: 链式处理成功值
  - `or_else(func)`: 链式处理错误值

## 十、通用与工具类型

### 1. `initializer_list`
- **构造与初始化**
  - 自动从花括号初始化列表构造
- **元素访问**
  - `begin()`: 返回指向第一个元素的指针
  - `end()`: 返回指向最后一个元素之后的指针
  - `size()`: 返回元素数量

### 2. `type_traits`
- **类型判断**
  - `is_same<T, U>`: 判断两个类型是否相同
  - `is_integral<T>`: 判断是否为整数类型
  - `is_floating_point<T>`: 判断是否为浮点类型
  - `is_pointer<T>`: 判断是否为指针类型
- **类型转换**
  - `remove_const<T>`: 移除const限定符
  - `add_pointer<T>`: 添加指针
  - `decay<T>`: 应用类型退化规则
- **条件类型**
  - `conditional<B, T, U>`: 根据条件选择类型

### 3. `regex`
- **正则表达式类**
  - `regex`: 表示正则表达式
  - `wregex`: 宽字符版本的regex
- **匹配结果类**
  - `smatch`: 字符串匹配结果
  - `wsmatch`: 宽字符串匹配结果
- **迭代器类**
  - `sregex_iterator`: 字符串正则迭代器
  - `wsregex_iterator`: 宽字符串正则迭代器
- **操作**
  - `regex_match`: 完全匹配
  - `regex_search`: 部分匹配
  - `regex_replace`: 替换匹配部分

### 4. `filesystem::path`, `directory_iterator`
- **`path`**
  - `path(const string& str)`: 从字符串构造路径
  - `filename()`: 返回文件名部分
  - `parent_path()`: 返回父路径
  - `extension()`: 返回扩展名
  - `exists()`: 检查路径是否存在
  - `is_directory()`: 检查是否为目录
  - `is_regular_file()`: 检查是否为常规文件
- **`directory_iterator`**
  - 迭代目录中的条目
  - `recursive_directory_iterator`: 递归迭代目录

### 5. `random`
- **随机数引擎**
  - `mt19937`: Mersenne Twister引擎
  - `random_device`: 非确定性随机数生成器
- **分布**
  - `uniform_int_distribution`: 均匀整数分布
  - `uniform_real_distribution`: 均匀实数分布
  - `normal_distribution`: 正态分布
  - `poisson_distribution`: 泊松分布
- **操作**
  - `engine()`: 生成随机数
  - `distribution(engine)`: 根据分布生成随机数

### 6. `numeric_limits`
- **操作**
  - `min()`: 返回类型的最小值
  - `max()`: 返回类型的最大值
  - `lowest()`: 返回类型的最小有限值
  - `digits`: 返回类型的位数
  - `is_signed`: 判断类型是否有符号
  - `is_integer`: 判断类型是否为整数类型

### 7. C++20 位操作函数
- `popcount(x)`: 计算x中1的个数
- `countl_zero(x)`: 计算前导零的个数
- `countr_zero(x)`: 计算末尾零的个数
- `bit_ceil(x)`: 返回不小于x的最小2的幂
- `bit_floor(x)`: 返回不大于x的最大2的幂

### 8. `source_location`
- **成员函数**
  - `file_name()`: 返回文件名
  - `line()`: 返回行号
  - `column()`: 返回列号
  - `function_name()`: 返回函数名
