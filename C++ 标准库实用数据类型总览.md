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
