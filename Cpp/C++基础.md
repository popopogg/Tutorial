## 第一阶段：语言基础（地基）

### 1.1 程序结构与编译模型

```
源文件(.cpp) → 预处理 → 编译 → 汇编 → 目标文件(.o) → 链接 → 可执行文件

关键概念：
├── 翻译单元 (Translation Unit)
├── 头文件 vs 源文件 (.h / .cpp)
├── #include 的本质（文本替换）
├── 声明 vs 定义 (ODR: One Definition Rule)
├── 编译器：g++ / clang++ / MSVC
└── 编译选项：-std=c++17 -Wall -Wextra -O2 -g
```

### 1.2 基本类型系统

```
// ===== 基本类型 =====
bool                        // true / false
char, char16_t, char32_t    // 字符
int, short, long, long long // 整数
float, double, long double  // 浮点
void                        // 空类型

// ===== 类型修饰 =====
signed / unsigned            // 符号性
const                        // 不可变
volatile                     // 禁止优化（少用）

// ===== 固定宽度类型（推荐）=====
#include <cstdint>
int8_t, int16_t, int32_t, int64_t
uint8_t, uint16_t, uint32_t, uint64_t
size_t                       // 无符号，表示大小/索引

// ===== 类型转换 =====
static_cast<int>(3.14)       // 编译期检查的转换（最常用）
const_cast<int*>(cp)         // 移除/添加 const
reinterpret_cast<char*>(ip)  // 底层比特重新解释（危险）
dynamic_cast<Derived*>(bp)   // 多态向下转型（运行期检查）
// 禁止使用 C 风格强转: (int)x
```

### 1.3 变量与初始化

```
// ===== C++17 推荐的初始化方式 =====

// 1. 直接初始化（基本类型）
int x = 42;
int y{42};            // 花括号初始化，禁止窄化转换 ✓

// 2. auto 类型推导
auto i = 42;          // int
auto d = 3.14;        // double
auto s = "hello"s;    // std::string（需要 using namespace std::literals）
auto v = std::vector{1, 2, 3};  // CTAD (C++17)

// 3. const 与 constexpr
const int MAX = 100;           // 运行期常量
constexpr int SIZE = 1024;     // 编译期常量（更强）
// 规则：能用 constexpr 就用 constexpr，否则用 const

// 4. 结构化绑定 (C++17)
auto [x, y] = std::pair{1, 2};
auto [key, value] = *myMap.begin();

// ===== 作用域与生命周期 =====
// 自动存储期（栈）：函数/块内局部变量
// 静态存储期：全局变量、static 变量
// 动态存储期（堆）：new/delete（应避免直接使用）
// 线程存储期：thread_local
```

### 1.4 控制流

```
// ===== 条件 =====
if (cond) { } else if (cond2) { } else { }

// C++17: if 带初始化
if (auto it = m.find(key); it != m.end()) {
    use(it->second);
}

switch (value) {
    case 1:  doA(); break;
    case 2:  doB(); [[fallthrough]]; // C++17 显式贯穿
    case 3:  doC(); break;
    default: doDefault();
}

// ===== 循环 =====
for (int i = 0; i < n; ++i) { }          // 经典 for
for (const auto& elem : container) { }    // 范围 for（优先使用）
while (cond) { }
do { } while (cond);

// 范围 for 的选择：
// for (auto elem : v)        — 拷贝（小类型/需要副本时）
// for (auto& elem : v)       — 可修改引用
// for (const auto& elem : v) — 只读引用（最常用）✓
```

### 1.5 函数

```
// ===== 基本定义 =====
int add(int a, int b) { return a + b; }

// ===== 参数传递规则 =====
void func(int x);               // 传值：基本类型、小对象
void func(const std::string& s); // const引用：大对象只读
void func(std::string& s);      // 引用：需要修改
void func(std::string s);       // 传值：需要拷贝一份（sink参数）

// ===== 返回值 =====
std::string make();              // 值返回（编译器 RVO/NRVO 优化，零拷贝）
// C++17 保证: 拷贝消除是强制的（guaranteed copy elision）

// ===== 默认参数 =====
void log(const std::string& msg, int level = 0);

// ===== 函数重载 =====
int    process(int x);
double process(double x);
// 编译器根据参数类型选择最匹配的版本

// ===== auto 返回类型 (C++14) =====
auto multiply(int a, int b) { return a * b; }

// ===== [[nodiscard]] (C++17) =====
[[nodiscard]] int compute();   // 调用者忽略返回值时警告
```

### 1.6 命名空间

```
namespace project::module::detail {  // C++17 嵌套命名空间
    void internal_func();
}

// 使用
project::module::detail::internal_func();

// 别名
namespace pm = project::module;

// using 声明（谨慎使用）
using std::vector;         // 引入单个名字
// using namespace std;    // ❌ 头文件中绝不使用，源文件中也尽量避免
```

## 第二阶段：面向对象（核心）

### 2.1 类的定义与封装

```
class Student {
public:
    // --- 构造函数 ---
    Student() = default;                                // 默认构造
    Student(std::string name, int age)                  // 参数化构造
        : name_(std::move(name)), age_(age) {}          // 成员初始化列表

    Student(std::string name)                           // 委托构造
        : Student(std::move(name), 0) {}

    // --- 析构函数 ---
    ~Student() = default;

    // --- 公共接口 ---
    const std::string& name() const { return name_; }   // getter
    void set_name(std::string name) { name_ = std::move(name); }  // setter

    void print() const {                                // const 成员函数
        std::cout << name_ << ", " << age_ << "\n";
    }

private:
    // --- 数据成员（类内默认初始化）---
    std::string name_ = "unknown";   // C++11 类内初始化
    int age_ = 0;
    inline static int count_ = 0;   // C++17 inline 静态成员
};

// class vs struct：唯一区别是默认访问权限
// class  → 默认 private
// struct → 默认 public（常用于纯数据聚合体 / POD）
```

### 2.2 特殊成员函数 —— Rule of 0 / 5

```
/*
 * 编译器可自动生成 6 个特殊成员函数：
 *   1. 默认构造函数
 *   2. 析构函数
 *   3. 拷贝构造函数
 *   4. 拷贝赋值运算符
 *   5. 移动构造函数
 *   6. 移动赋值运算符
 *
 * ★ Rule of Zero:
 *   如果你的类只使用 RAII 类型成员（string, vector, unique_ptr...），
 *   → 不需要手写任何特殊成员函数，编译器生成的就是正确的
 *
 * ★ Rule of Five:
 *   如果你必须手写其中一个（如管理裸资源），→ 全部五个都要写
 */

// ===== Rule of Zero 示例（推荐）=====
class Person {
    std::string name_;
    std::vector<int> scores_;
    std::unique_ptr<Detail> detail_;
    // 不需要写任何特殊成员函数！编译器自动正确处理
};

// ===== Rule of Five 示例（仅在管理裸资源时）=====
class Buffer {
public:
    explicit Buffer(size_t size)
        : data_(new int[size]), size_(size) {}

    ~Buffer() { delete[] data_; }                       // 1. 析构

    Buffer(const Buffer& other)                         // 2. 拷贝构造
        : data_(new int[other.size_]), size_(other.size_) {
        std::copy(other.data_, other.data_ + size_, data_);
    }

    Buffer& operator=(const Buffer& other) {            // 3. 拷贝赋值
        if (this != &other) {
            Buffer tmp(other);          // copy-and-swap idiom
            swap(tmp);
        }
        return *this;
    }

    Buffer(Buffer&& other) noexcept                     // 4. 移动构造
        : data_(std::exchange(other.data_, nullptr)),
          size_(std::exchange(other.size_, 0)) {}

    Buffer& operator=(Buffer&& other) noexcept {        // 5. 移动赋值
        if (this != &other) {
            delete[] data_;
            data_ = std::exchange(other.data_, nullptr);
            size_ = std::exchange(other.size_, 0);
        }
        return *this;
    }

    void swap(Buffer& other) noexcept {
        std::swap(data_, other.data_);
        std::swap(size_, other.size_);
    }

private:
    int* data_ = nullptr;
    size_t size_ = 0;
};

// 禁用拷贝
class NonCopyable {
public:
    NonCopyable(const NonCopyable&) = delete;
    NonCopyable& operator=(const NonCopyable&) = delete;
};
```

### 2.3 继承与多态

```
// ===== 基类 =====
class Shape {
public:
    virtual ~Shape() = default;                    // ★ 虚析构函数
    virtual double area() const = 0;               // 纯虚函数 → 抽象类
    virtual void draw() const { /* 默认实现 */ }
};

// ===== 派生类 =====
class Circle : public Shape {
public:
    explicit Circle(double r) : radius_(r) {}

    double area() const override {                 // ★ override 强制检查
        return 3.14159 * radius_ * radius_;
    }

    void draw() const override { /* ... */ }

private:
    double radius_;
};

class Square final : public Shape {                // final 禁止继续继承
public:
    explicit Square(double s) : side_(s) {}
    double area() const override { return side_ * side_; }
    void draw() const final { /* ... */ }          // final 禁止覆盖

private:
    double side_;
};

// ===== 多态使用 =====
void print_area(const Shape& shape) {              // 通过引用/指针实现多态
    std::cout << shape.area() << "\n";
}

Circle c(5.0);
Square s(3.0);
print_area(c);  // 调用 Circle::area()
print_area(s);  // 调用 Square::area()

// 用智能指针管理多态对象
std::vector<std::unique_ptr<Shape>> shapes;
shapes.push_back(std::make_unique<Circle>(5.0));
shapes.push_back(std::make_unique<Square>(3.0));
for (const auto& shape : shapes) {
    std::cout << shape->area() << "\n";
}

/*
 * 继承要点：
 * 1. 基类析构函数必须是 virtual（否则通过基类指针 delete 导致未定义行为）
 * 2. 派生类重写时必须加 override
 * 3. 优先使用组合而非继承 (Composition over Inheritance)
 * 4. 接口继承（纯虚函数）比实现继承更安全
 */
```

### 2.4 运算符重载

```
class Vec2 {
public:
    double x = 0, y = 0;

    // 算术运算符（成员函数）
    Vec2 operator+(const Vec2& rhs) const { return {x + rhs.x, y + rhs.y}; }
    Vec2 operator-(const Vec2& rhs) const { return {x - rhs.x, y - rhs.y}; }
    Vec2 operator*(double s) const { return {x * s, y * s}; }
    Vec2& operator+=(const Vec2& rhs) { x += rhs.x; y += rhs.y; return *this; }

    // 比较运算符 — 手动写（C++20 可用 <=>）
    bool operator==(const Vec2& rhs) const { return x == rhs.x && y == rhs.y; }
    bool operator!=(const Vec2& rhs) const { return !(*this == rhs); }

    // 下标运算符
    double& operator[](int i) { return i == 0 ? x : y; }
    const double& operator[](int i) const { return i == 0 ? x : y; }

    // 函数调用运算符（函数对象）
    double operator()(double t) const { return x * t + y; }
};

// 友元（非成员函数）— 用于左操作数非本类的情况
Vec2 operator*(double s, const Vec2& v) { return v * s; }

std::ostream& operator<<(std::ostream& os, const Vec2& v) {
    return os << "(" << v.x << ", " << v.y << ")";
}

// 用户定义字面量
Vec2 operator""_x(long double val) { return {static_cast<double>(val), 0}; }
auto v = 3.0_x;  // Vec2{3.0, 0}
```

## 第三阶段：资源管理与值语义

### 3.1 RAII（资源获取即初始化）

```
核心原则：
  资源的生命周期绑定到对象的生命周期
  构造时获取 → 析构时释放 → 栈展开自动清理

资源类型：        RAII 包装器：
  堆内存            unique_ptr / shared_ptr
  文件句柄          ifstream / ofstream
  互斥锁            lock_guard / unique_lock / scoped_lock
  网络连接          自定义 RAII 类
  数据库事务        自定义 RAII 类

关键好处：
  ✓ 异常安全 — 即使抛异常，析构函数也会被调用
  ✓ 无泄漏   — 不需要手动释放
  ✓ 简洁     — 不需要 try-finally
```

### 3.2 智能指针（详解）

```
#include <memory>

// ===== unique_ptr：独占所有权 =====
// 最常用，零开销抽象（和裸指针一样大、一样快）
auto p = std::make_unique<MyClass>(arg1, arg2);  // ★ 创建方式
p->method();
*p;                          // 解引用
p.get();                     // 获取裸指针（不转移所有权）
p.reset();                   // 释放并置空
p.reset(new MyClass());      // 释放旧的，持有新的
auto p2 = std::move(p);     // 转移所有权（p 变为 nullptr）

// 自定义删除器
auto file_deleter = [](FILE* fp) { if (fp) fclose(fp); };
std::unique_ptr<FILE, decltype(file_deleter)> file(
    fopen("data.txt", "r"), file_deleter
);

// 数组形式
auto arr = std::make_unique<int[]>(100);
arr[0] = 42;

// ===== shared_ptr：共享所有权（引用计数）=====
auto sp1 = std::make_shared<MyClass>(args...);  // ★ 优先用 make_shared
auto sp2 = sp1;              // 引用计数 = 2
sp1.reset();                 // 引用计数 = 1
sp2.reset();                 // 引用计数 = 0 → 自动删除

sp1.use_count();             // 查看引用计数（调试用）

// ⚠ 循环引用问题
struct Node {
    std::shared_ptr<Node> next;  // A→B, B→A → 内存泄漏！
};

// ===== weak_ptr：解决循环引用 =====
struct Node {
    std::shared_ptr<Node> next;
    std::weak_ptr<Node> prev;    // ★ 弱引用，不增加计数
};

std::weak_ptr<MyClass> wp = sp1;
if (auto locked = wp.lock()) {   // 提升为 shared_ptr
    locked->method();            // 对象仍存活
}
wp.expired();                    // 检查对象是否已销毁

/*
 * 选择指南：
 * ┌─────────────────────────────────────┐
 * │ 情况                 │ 选择         │
 * ├─────────────────────────────────────┤
 * │ 独占拥有             │ unique_ptr   │ ← 默认选择
 * │ 共享拥有             │ shared_ptr   │
 * │ 观察但不拥有         │ weak_ptr     │
 * │ 不拥有的函数参数     │ T& 或 T*    │
 * │ 绝不需要堆分配       │ 栈上对象     │
 * └─────────────────────────────────────┘
 */
```

### 3.3 移动语义（深入理解）

```
/*
 * ===== 值类别 (Value Categories) =====
 *
 *            expression
 *           /          \
 *        glvalue      rvalue
 *       /      \     /     \
 *    lvalue   xvalue    prvalue
 *
 * lvalue:  有名字，可取地址        (变量、解引用)
 * prvalue: 无名字，不可取地址      (字面量、临时对象)
 * xvalue:  将亡值                 (std::move 的结果)
 */

// ===== 什么时候发生移动？ =====
std::string s1 = "hello";
std::string s2 = s1;               // 拷贝（s1 是左值）
std::string s3 = std::move(s1);    // 移动（std::move 将 s1 转为右值引用）
std::string s4 = getName();        // 移动或 RVO（临时对象是右值）

// std::move 本身不移动任何东西！
// 它只是一个类型转换：static_cast<T&&>(x)
// 真正的移动发生在移动构造/移动赋值函数里

// ===== 移动后的状态 =====
// 被移动的对象处于 "有效但未指定" 状态
// 唯一安全操作：赋新值 或 销毁
std::string moved_from = "hello";
std::string target = std::move(moved_from);
// moved_from 现在可能是空串，但不保证
moved_from = "new value";   // ✓ 可以重新赋值

// ===== 何时使用 std::move =====
// ✓ 明确不再需要某个对象时
// ✓ 函数返回局部变量时 → 不需要！编译器自动 RVO
// ✗ 不要 move const 对象（会退化为拷贝）
// ✗ 不要 move 返回的局部变量（阻碍 RVO）

std::string make() {
    std::string result = "hello";
    return result;              // ✓ 自动 RVO，不要写 std::move(result)
}
```

### 3.4 完美转发

```
/*
 * 问题：如何写一个函数，把参数原封不动地传给另一个函数？
 *       保持：值类别（左值/右值）、const/volatile 修饰
 *
 * 答案：万能引用 + std::forward
 */

// T&& 在模板参数推导中是 "万能引用" (forwarding reference)
// 不是右值引用！

template<typename T>
void wrapper(T&& arg) {
    // 引用折叠规则：
    // T = int&   → int& &&  → int&   (左值)
    // T = int    → int&&            (右值)

    target(std::forward<T>(arg));   // 完美转发
}

// 经典应用：工厂函数
template<typename T, typename... Args>
std::unique_ptr<T> make_unique_v2(Args&&... args) {
    return std::unique_ptr<T>(new T(std::forward<Args>(args)...));
}

// emplace 系列函数就是靠完美转发实现原地构造
vec.emplace_back(arg1, arg2);  // 直接在容器内构造，避免临时对象
```

## 第四阶段：标准库（STL）

### 4.1 容器

```
STL 容器分类：

┌─────────────── 序列容器 ───────────────┐
│ array<T,N>   固定大小数组              │ O(1) 随机访问
│ vector<T>    动态数组（★最常用）        │ O(1) 随机访问, 尾部O(1)均摊
│ deque<T>     双端队列                  │ O(1) 随机访问, 头尾O(1)
│ list<T>      双向链表                  │ O(1) 任意插删, 无随机访问
│ forward_list 单向链表                  │ 同上，更省内存
└────────────────────────────────────────┘

┌─────────── 关联容器（红黑树）───────────┐
│ set<T>          有序集合               │ O(log n) 查找/插入/删除
│ map<K,V>        有序键值对（★常用）     │ O(log n)
│ multiset<T>     允许重复的有序集合      │
│ multimap<K,V>   允许重复键的有序映射    │
└────────────────────────────────────────┘

┌──────── 无序关联容器（哈希表）──────────┐
│ unordered_set<T>      哈希集合         │ O(1) 平均
│ unordered_map<K,V>    哈希映射（★常用） │ O(1) 平均
│ unordered_multiset                     │
│ unordered_multimap                     │
└────────────────────────────────────────┘

┌─────────── 容器适配器 ─────────────────┐
│ stack<T>         栈 LIFO               │
│ queue<T>         队列 FIFO             │
│ priority_queue<T> 优先队列（堆）        │
└────────────────────────────────────────┘
```



```
// ===== vector 详解（最重要的容器）=====
#include <vector>

std::vector<int> v;
std::vector<int> v{1, 2, 3, 4, 5};     // 初始化列表
std::vector<int> v(10, 0);              // 10 个 0
std::vector v = {1, 2, 3};              // CTAD (C++17)

// 容量操作
v.size();               // 当前元素数
v.capacity();           // 已分配容量
v.empty();              // 是否为空
v.reserve(100);         // ★ 预分配空间（避免多次扩容）
v.shrink_to_fit();      // 释放多余空间

// 添加元素
v.push_back(42);                  // 拷贝/移动到尾部
v.emplace_back(arg1, arg2);      // ★ 原地构造（更高效）
v.insert(v.begin() + 2, 99);     // 在位置 2 插入

// 删除元素
v.pop_back();                     // 删除尾部
v.erase(v.begin() + 2);          // 删除位置 2
v.erase(v.begin(), v.begin()+3); // 删除范围
v.clear();                        // 清空

// 访问
v[0];                // 无边界检查
v.at(0);             // 有边界检查（越界抛 out_of_range）
v.front();           // 第一个
v.back();            // 最后一个
v.data();            // 底层数组指针

// ===== map 详解 =====
#include <map>

std::map<std::string, int> scores;
scores["Alice"] = 95;
scores.insert({"Bob", 88});
scores.emplace("Charlie", 92);
scores.insert_or_assign("Alice", 98);        // C++17: 存在则更新
scores.try_emplace("Dave", 85);              // C++17: 不存在才插入

// 查找
if (auto it = scores.find("Alice"); it != scores.end()) {
    std::cout << it->second;
}
if (scores.contains("Alice")) { /* C++20, C++17 用 count */ }
if (scores.count("Alice") > 0) { /* C++17 */ }

// 遍历
for (const auto& [name, score] : scores) {   // 结构化绑定
    std::cout << name << ": " << score << "\n";
}

// ⚠ operator[] 的陷阱：访问不存在的键会自动插入默认值
int val = scores["Unknown"];  // 插入 {"Unknown", 0}！
```

### 4.2 迭代器

```
/*
 * 迭代器类别（从弱到强）：
 *
 * InputIterator         → 只读，单遍，前进
 * OutputIterator        → 只写，单遍，前进
 * ForwardIterator       → 读写，多遍，前进
 * BidirectionalIterator → 读写，多遍，双向    (list, set, map)
 * RandomAccessIterator  → 读写，多遍，随机跳转 (vector, deque, array)
 */

std::vector<int> v = {10, 20, 30, 40, 50};

auto it = v.begin();     // 指向第一个元素
auto end = v.end();      // 指向尾后位置

*it;          // 解引用 → 10
++it;         // 前进
--it;         // 后退（双向迭代器）
it += 3;      // 跳转（随机访问迭代器）
it[2];        // 随机访问

// 反向迭代器
for (auto rit = v.rbegin(); rit != v.rend(); ++rit) { /* 反向遍历 */ }

// const 迭代器
auto cit = v.cbegin();  // const_iterator，不能通过它修改元素
```

### 4.3 算法库

```
#include <algorithm>
#include <numeric>

std::vector<int> v = {5, 3, 1, 4, 2};

// ===== 排序 =====
std::sort(v.begin(), v.end());                    // 升序
std::sort(v.begin(), v.end(), std::greater<>{}); // 降序
std::sort(v.begin(), v.end(), [](int a, int b) {
    return a > b;                                 // 自定义比较
});
std::stable_sort(v.begin(), v.end());             // 稳定排序
std::partial_sort(v.begin(), v.begin()+3, v.end()); // 前 3 最小
std::nth_element(v.begin(), v.begin()+2, v.end());  // 第 3 小的元素就位

// ===== 查找 =====
auto it = std::find(v.begin(), v.end(), 3);                 // 线性查找
auto it = std::find_if(v.begin(), v.end(), [](int x) {      // 条件查找
    return x > 3;
});
bool found = std::binary_search(v.begin(), v.end(), 3);     // 二分查找（需有序）
auto it = std::lower_bound(v.begin(), v.end(), 3);          // 第一个 >= 3
auto it = std::upper_bound(v.begin(), v.end(), 3);          // 第一个 > 3

// ===== 变换 =====
std::transform(v.begin(), v.end(), v.begin(), [](int x) {
    return x * 2;
});
std::for_each(v.begin(), v.end(), [](int x) {
    std::cout << x << " ";
});

// ===== 过滤/删除 =====
// Erase-Remove Idiom
v.erase(std::remove_if(v.begin(), v.end(),
    [](int x) { return x % 2 == 0; }), v.end());

// ===== 聚合 =====
int sum = std::accumulate(v.begin(), v.end(), 0);
int product = std::accumulate(v.begin(), v.end(), 1, std::multiplies<>{});

auto [min_it, max_it] = std::minmax_element(v.begin(), v.end());
int clamped = std::clamp(value, low, high);  // C++17

// ===== 判断 =====
bool all  = std::all_of(v.begin(), v.end(), pred);
bool any  = std::any_of(v.begin(), v.end(), pred);
bool none = std::none_of(v.begin(), v.end(), pred);

// ===== 拷贝/生成 =====
std::copy(src.begin(), src.end(), std::back_inserter(dst));
std::copy_if(src.begin(), src.end(), std::back_inserter(dst), pred);
std::fill(v.begin(), v.end(), 0);
std::generate(v.begin(), v.end(), [n=0]() mutable { return n++; });
std::iota(v.begin(), v.end(), 1);  // {1, 2, 3, ...}

// ===== 集合操作（需有序）=====
std::set_union, std::set_intersection, std::set_difference
```

### 4.4 字符串处理

```
#include <string>
#include <string_view>    // C++17

// ===== std::string =====
std::string s = "Hello, World!";
s.size();  s.length();     // 长度
s.empty();                 // 是否为空
s.substr(7, 5);            // "World"
s.find("World");           // 位置，找不到返回 npos
s.rfind("l");              // 反向查找
s.replace(7, 5, "C++");   // 替换
s.starts_with("Hello");   // C++20 (C++17 用 s.find("Hello") == 0)
s.ends_with("!");          // C++20

s += " Goodbye";           // 追加
s.append(" End");
s.insert(5, "!!!");

// 数值转换
int n = std::stoi("42");
double d = std::stod("3.14");
std::string ns = std::to_string(42);

// ===== std::string_view（C++17 ★重要）=====
// 非拥有的字符串视图：不分配内存，不拷贝
// 可以看作 {const char* data, size_t size} 的封装

void process(std::string_view sv) {     // ★ 函数参数优先用 string_view
    sv.substr(0, 5);                    // 返回 string_view，零拷贝
    sv.find("hello");
    sv.remove_prefix(2);
    sv.remove_suffix(3);
}

process("literal");              // ✓ 无需构造 string
process(some_string);            // ✓ 无拷贝
process(some_string_view);       // ✓

// ⚠ 注意：string_view 不拥有数据，不能比源字符串活得更长
// std::string_view dangling() {
//     std::string s = "temp";
//     return s;  // ❌ 悬垂引用！
// }
```

## 第五阶段：模板编程

### 5.1 函数模板

```
// 基本函数模板
template<typename T>
T max_of(T a, T b) {
    return (a > b) ? a : b;
}

// 调用
max_of(3, 5);           // T = int（自动推导）
max_of<double>(3, 5.0); // 显式指定

// 多类型参数
template<typename T, typename U>
auto add(T a, U b) {
    return a + b;        // 返回类型自动推导
}

// 非类型模板参数
template<typename T, int N>
struct Array {
    T data[N];
    constexpr int size() const { return N; }
};
Array<int, 10> arr;
```

### 5.2 类模板

```
template<typename T>
class Stack {
public:
    void push(const T& value) { data_.push_back(value); }
    void push(T&& value) { data_.push_back(std::move(value)); }

    template<typename... Args>
    void emplace(Args&&... args) {
        data_.emplace_back(std::forward<Args>(args)...);
    }

    T& top() { return data_.back(); }
    const T& top() const { return data_.back(); }
    void pop() { data_.pop_back(); }
    bool empty() const { return data_.empty(); }
    size_t size() const { return data_.size(); }

private:
    std::vector<T> data_;
};

// C++17 CTAD（类模板参数推导）
Stack s;          // ❌ 不行，无法推导
Stack s2 = Stack<int>{};  // 显式
// 需要推导指引(deduction guide)或初始参数
```

### 5.3 变参模板

```
// ===== 递归展开（C++11）=====
// 基础情况
void print() { std::cout << "\n"; }

// 递归情况
template<typename T, typename... Args>
void print(T first, Args... rest) {
    std::cout << first << " ";
    print(rest...);
}

print(1, "hello", 3.14);  // "1 hello 3.14\n"

// ===== 折叠表达式（C++17 ★ 更简洁）=====
template<typename... Args>
auto sum(Args... args) {
    return (args + ...);          // 一元右折叠: a1 + (a2 + (a3 + a4))
}

template<typename... Args>
auto sum2(Args... args) {
    return (... + args);          // 一元左折叠: ((a1 + a2) + a3) + a4
}

template<typename... Args>
auto sum3(Args... args) {
    return (0 + ... + args);      // 二元左折叠（带初始值）
}

template<typename... Args>
void print(Args... args) {
    ((std::cout << args << " "), ...);   // 逗号折叠，依次输出
    std::cout << "\n";
}

// sizeof... 获取参数包大小
template<typename... Args>
constexpr auto count = sizeof...(Args);
```

### 5.4 模板特化

```
// ===== 完全特化 =====
template<typename T>
struct TypeName {
    static constexpr const char* name = "unknown";
};

template<>
struct TypeName<int> {
    static constexpr const char* name = "int";
};

template<>
struct TypeName<double> {
    static constexpr const char* name = "double";
};

// ===== 偏特化（部分特化）=====
// 只对类模板有效，函数模板不能偏特化

template<typename T>
struct TypeName<std::vector<T>> {
    static constexpr const char* name = "vector";
};

template<typename T>
struct TypeName<T*> {
    static constexpr const char* name = "pointer";
};
```

### 5.5 SFINAE 与 enable_if

```
// SFINAE = Substitution Failure Is Not An Error
// 替换失败不是错误 → 编译器会尝试其他重载

#include <type_traits>

// C++11/14 风格
template<typename T>
std::enable_if_t<std::is_integral_v<T>, T>
double_val(T x) {
    return x * 2;
}

template<typename T>
std::enable_if_t<std::is_floating_point_v<T>, T>
double_val(T x) {
    return x * 2.0;
}

// ===== C++17 更优方案：if constexpr =====
template<typename T>
auto double_val(T x) {
    if constexpr (std::is_integral_v<T>) {
        return x * 2;
    } else if constexpr (std::is_floating_point_v<T>) {
        return x * 2.0;
    } else {
        static_assert(sizeof(T) == 0, "Unsupported type");
        // 依赖 T 的 static_assert，只有该分支被实例化时才会触发
    }
}

// ===== 常用 type_traits =====
std::is_integral_v<T>          // 整数类型？
std::is_floating_point_v<T>    // 浮点类型？
std::is_same_v<T, U>           // 相同类型？
std::is_base_of_v<Base, D>     // 继承关系？
std::is_constructible_v<T, Args...>
std::is_trivially_copyable_v<T>
std::is_invocable_v<F, Args...>

std::remove_reference_t<T>     // 移除引用
std::remove_cv_t<T>            // 移除 const/volatile
std::decay_t<T>                // 衰变（数组→指针, 函数→函数指针, 移除cv和引用）
std::conditional_t<cond, T, F> // 编译期条件选择
std::common_type_t<T, U>       // 公共类型
```

## 第六阶段：Lambda 与函数式编程

### 6.1 Lambda 完全指南

```
// 完整语法：
// [captures] (params) specifiers -> return_type { body }

// ===== 捕获方式 =====
int x = 10;
std::string s = "hello";

auto f1 = [x]()     { return x; };        // 值捕获（拷贝）
auto f2 = [&x]()    { x++; };             // 引用捕获
auto f3 = [=]()     { return x + 1; };    // 值捕获全部
auto f4 = [&]()     { x++; };             // 引用捕获全部
auto f5 = [=, &s]() { return s + "!"; };  // 默认值，s引用
auto f6 = [&, x]()  { return x; };        // 默认引用，x值

// C++14: 初始化捕获（广义捕获）
auto f7 = [y = x * 2]() { return y; };             // 新变量
auto f8 = [s = std::move(s)]() { return s; };       // 移动捕获
auto f9 = [p = std::make_unique<int>(42)]() {       // 独占指针
    return *p;
};

// mutable: 允许修改值捕获的变量
auto counter = [n = 0]() mutable { return ++n; };
counter();  // 1
counter();  // 2

// ===== 泛型 Lambda (C++14) =====
auto print = [](const auto& x) { std::cout << x << "\n"; };
auto add = [](auto a, auto b) { return a + b; };

// ===== Lambda 与 STL 配合 =====
std::vector<int> v = {5, 3, 1, 4, 2};

std::sort(v.begin(), v.end(), [](int a, int b) { return a > b; });

auto it = std::find_if(v.begin(), v.end(), [](int x) { return x > 3; });

std::transform(v.begin(), v.end(), v.begin(),
    [factor = 2](int x) { return x * factor; });

int count = std::count_if(v.begin(), v.end(),
    [](int x) { return x % 2 == 0; });

v.erase(std::remove_if(v.begin(), v.end(),
    [](int x) { return x < 3; }), v.end());

// ===== IIFE: 立即调用的 Lambda =====
// 用于复杂初始化 const 变量
const auto config = [&]() {
    Config c;
    c.load("file.cfg");
    c.validate();
    return c;
}();  // 注意末尾的 ()
```

### 6.2 std::function 与回调

```
#include <functional>

// std::function: 通用函数包装器
std::function<int(int, int)> op;

op = [](int a, int b) { return a + b; };   // Lambda
op = std::plus<int>{};                      // 函数对象
op = &free_function;                        // 函数指针

int result = op(3, 4);

// 作为回调参数
void process(const std::vector<int>& data,
             std::function<void(int)> callback) {
    for (int x : data) callback(x);
}

// ⚠ 性能注意：std::function 有类型擦除开销
// 在模板中，直接用模板参数接收可调用对象更高效
template<typename Func>
void process(const std::vector<int>& data, Func&& callback) {
    for (int x : data) callback(x);
}

// std::invoke: 统一调用语法 (C++17)
std::invoke(func, args...);      // 普通函数/Lambda
std::invoke(&Class::method, obj, args...);  // 成员函数
std::invoke(&Class::member, obj);           // 成员变量
```

## 第七阶段：C++17 专属特性

### 7.1 std::optional

```
#include <optional>

// 表示 "可能有值，也可能没有"，替代 "返回特殊值" 或 "输出参数"

std::optional<int> find_index(const std::vector<int>& v, int target) {
    for (int i = 0; i < v.size(); ++i) {
        if (v[i] == target) return i;      // 有值
    }
    return std::nullopt;                    // 无值
}

// 使用
auto result = find_index(v, 42);
if (result) {                               // 或 result.has_value()
    std::cout << *result << "\n";           // 解引用
    std::cout << result.value() << "\n";    // 有边界检查
}
int val = result.value_or(-1);              // 提供默认值
```

### 7.2 std::variant

```
#include <variant>

// 类型安全的联合体：同一时刻只持有一个类型的值

std::variant<int, double, std::string> v;
v = 42;                  // 持有 int
v = 3.14;                // 持有 double
v = "hello"s;            // 持有 string

// 获取值
std::get<int>(v);                 // 类型不匹配则抛 bad_variant_access
std::get<2>(v);                   // 按索引
auto* p = std::get_if<int>(&v);  // 返回指针，类型不匹配返回 nullptr

v.index();                        // 当前持有类型的索引

// ★ std::visit — 访问者模式
std::visit([](auto&& arg) {
    std::cout << arg << "\n";
}, v);

// 多 variant 同时访问
std::visit([](auto&& a, auto&& b) {
    // 处理所有组合
}, v1, v2);

// overloaded 技巧（常用模式）
template<typename... Ts>
struct overloaded : Ts... { using Ts::operator()...; };
template<typename... Ts>
overloaded(Ts...) -> overloaded<Ts...>;  // 推导指引

std::visit(overloaded{
    [](int val)         { std::cout << "int: " << val; },
    [](double val)      { std::cout << "double: " << val; },
    [](const std::string& val) { std::cout << "string: " << val; },
}, v);
```

### 7.3 std::any

```
#include <any>

std::any a = 42;
a = std::string("hello");
a = 3.14;

// 获取值
auto val = std::any_cast<double>(a);    // 类型不匹配抛 bad_any_cast
auto* p = std::any_cast<double>(&a);   // 返回指针

a.has_value();
a.type();       // 返回 std::type_info
a.reset();      // 清空

// 适用场景：类型完全未知的容器（如插件系统）
// 大多数情况 variant 更好（编译期类型检查）
```

### 7.4 结构化绑定

```
// 数组
int arr[] = {1, 2, 3};
auto [a, b, c] = arr;

// pair/tuple
auto [first, second] = std::make_pair(1, "hello");
auto [x, y, z] = std::make_tuple(1, 2.0, "three");

// struct
struct Point { double x, y; };
auto [px, py] = Point{3.0, 4.0};

// map 遍历（最实用）
std::map<std::string, int> scores = {{"Alice", 95}, {"Bob", 88}};
for (const auto& [name, score] : scores) {
    std::cout << name << ": " << score << "\n";
}

// 与 if 结合
if (auto [it, inserted] = map.insert({"key", 42}); inserted) {
    std::cout << "插入成功\n";
}
```

### 7.5 if constexpr

```
// 编译期条件分支 — 未选中的分支不会被实例化

template<typename T>
std::string to_string(T value) {
    if constexpr (std::is_same_v<T, std::string>) {
        return value;
    } else if constexpr (std::is_arithmetic_v<T>) {
        return std::to_string(value);
    } else if constexpr (std::is_same_v<T, const char*>) {
        return std::string(value);
    } else {
        static_assert(sizeof(T) == 0, "Unsupported type");
    }
}

// 替代 SFINAE / enable_if，更易读
```

### 7.6 std::filesystem

```
#include <filesystem>
namespace fs = std::filesystem;

// 路径操作
fs::path p = "/home/user/project/main.cpp";
p.parent_path();     // "/home/user/project"
p.filename();        // "main.cpp"
p.stem();            // "main"
p.extension();       // ".cpp"
p / "subdir";        // 路径拼接

// 文件系统查询
fs::exists(p);
fs::is_regular_file(p);
fs::is_directory(p);
fs::file_size(p);
fs::last_write_time(p);

// 文件操作
fs::create_directories("a/b/c");
fs::copy_file("src.txt", "dst.txt", fs::copy_options::overwrite_existing);
fs::remove("file.txt");
fs::remove_all("directory");
fs::rename("old.txt", "new.txt");

// 目录遍历
for (const auto& entry : fs::directory_iterator(".")) {
    std::cout << entry.path() << "\n";
}
// 递归遍历
for (const auto& entry : fs::recursive_directory_iterator(".")) {
    if (entry.is_regular_file() && entry.path().extension() == ".cpp") {
        std::cout << entry.path() << "\n";
    }
}
```

### 7.7 并行算法

```
#include <algorithm>
#include <execution>

std::vector<int> v(1'000'000);

// 执行策略
std::sort(std::execution::seq, v.begin(), v.end());       // 顺序（默认）
std::sort(std::execution::par, v.begin(), v.end());       // 并行
std::sort(std::execution::par_unseq, v.begin(), v.end()); // 并行+向量化

// 适用于大多数算法
std::for_each(std::execution::par, v.begin(), v.end(), func);
std::transform(std::execution::par, v.begin(), v.end(), v.begin(), func);
std::reduce(std::execution::par, v.begin(), v.end());  // 并行求和
```

### 7.8 其他 C++17 特性汇总

```
// 内联变量 — 头文件中定义变量不违反 ODR
inline int global_val = 42;
struct Config {
    inline static const std::string name = "app";
};

// 嵌套命名空间
namespace A::B::C { /* ... */ }

// [[nodiscard]]
[[nodiscard]] ErrorCode process();

// [[maybe_unused]]
[[maybe_unused]] int debug_var = 42;

// std::clamp
int clamped = std::clamp(value, 0, 100);  // 限制在 [0, 100]

// std::apply — 将 tuple 展开为函数参数
auto t = std::make_tuple(1, 2.0, "three");
std::apply([](auto... args) { ((std::cout << args << " "), ...); }, t);

// std::invoke — 统一调用
std::invoke(func, args...);

// std::from_chars / std::to_chars — 高性能数值<->字符串转换
#include <charconv>
int val;
auto [ptr, ec] = std::from_chars(str.data(), str.data() + str.size(), val);
if (ec == std::errc{}) { /* 成功 */ }
```

## 第八阶段：并发编程

### 8.1 线程基础

```
#include <thread>

// 创建线程
void work(int id) { /* ... */ }

std::thread t1(work, 1);                           // 函数 + 参数
std::thread t2([]() { /* ... */ });                // Lambda
std::thread t3([](int x) { /* ... */ }, 42);       // Lambda + 参数

// ★ 必须 join 或 detach，否则析构时 std::terminate
t1.join();      // 等待线程结束
t2.detach();    // 放后台运行（通常不推荐）

// 线程信息
std::thread::hardware_concurrency();  // CPU 核心数
std::this_thread::get_id();
std::this_thread::sleep_for(std::chrono::milliseconds(100));
std::this_thread::yield();

// ⚠ 注意：传引用需要 std::ref
void modify(int& x) { x = 42; }
int val = 0;
std::thread t(modify, std::ref(val));  // 必须用 std::ref
t.join();
```

### 8.2 互斥量与锁

```
#include <mutex>
#include <shared_mutex>

// ===== 互斥量类型 =====
std::mutex mtx;                   // 基本互斥量
std::recursive_mutex rmtx;        // 允许同一线程多次加锁
std::timed_mutex tmtx;            // 支持超时
std::shared_mutex smtx;           // 读写锁 (C++17)

// ===== RAII 锁（永远不要手动 lock/unlock）=====

// lock_guard: 最简单，构造加锁，析构解锁
{
    std::lock_guard<std::mutex> lock(mtx);  // 或 C++17: std::lock_guard lock(mtx);
    // 临界区...
}  // 自动解锁

// unique_lock: 更灵活（可提前解锁、延迟加锁、条件变量配合）
{
    std::unique_lock<std::mutex> lock(mtx);
    // 临界区...
    lock.unlock();       // 提前解锁
    // 非临界区...
    lock.lock();         // 重新加锁
}

// scoped_lock: 同时锁多个互斥量，无死锁 (C++17 ★)
{
    std::scoped_lock lock(mtx1, mtx2, mtx3);
    // 安全访问多个共享资源
}

// shared_lock: 读锁 (C++17)
{
    std::shared_lock lock(smtx);   // 多个读者可同时持有
    // 读操作...
}
{
    std::unique_lock lock(smtx);   // 写锁，独占
    // 写操作...
}

// ===== 单次初始化 =====
std::once_flag flag;
std::call_once(flag, []() {
    // 线程安全的一次性初始化
});
```

### 8.3 条件变量

```
#include <condition_variable>

// 生产者-消费者模型
std::mutex mtx;
std::condition_variable cv;
std::queue<int> buffer;
bool done = false;

// 生产者
void producer() {
    for (int i = 0; i < 100; ++i) {
        {
            std::lock_guard lock(mtx);
            buffer.push(i);
        }
        cv.notify_one();    // 通知一个等待的消费者
    }
    {
        std::lock_guard lock(mtx);
        done = true;
    }
    cv.notify_all();        // 通知所有消费者
}

// 消费者
void consumer() {
    while (true) {
        std::unique_lock lock(mtx);     // 条件变量必须用 unique_lock
        cv.wait(lock, [&]() {
            return !buffer.empty() || done;  // 等待条件（防止虚假唤醒）
        });
        while (!buffer.empty()) {
            int val = buffer.front();
            buffer.pop();
            lock.unlock();
            process(val);                // 处理数据（不持锁）
            lock.lock();
        }
        if (done) break;
    }
}
```

### 8.4 异步编程

```
#include <future>

// ===== std::async =====
auto future = std::async(std::launch::async, [](int x) {
    return x * x;
}, 42);

int result = future.get();  // 阻塞等待结果（只能 get 一次）

// 策略
std::launch::async     // 新线程执行
std::launch::deferred  // 延迟到 get()/wait() 时在当前线程执行
std::launch::async | std::launch::deferred  // 默认，由实现选择

// ===== promise + future（手动控制）=====
std::promise<int> prom;
std::future<int> fut = prom.get_future();

std::thread t([&prom]() {
    try {
        int result = compute();
        prom.set_value(result);           // 设置结果
    } catch (...) {
        prom.set_exception(std::current_exception());  // 传递异常
    }
});

int val = fut.get();  // 阻塞等待，如果有异常会重新抛出
t.join();

// ===== packaged_task =====
std::packaged_task<int(int, int)> task([](int a, int b) {
    return a + b;
});
auto fut = task.get_future();
std::thread t(std::move(task), 3, 4);
int result = fut.get();  // 7
t.join();
```

### 8.5 原子操作

```
#include <atomic>

std::atomic<int> counter{0};
std::atomic<bool> flag{false};

// 基本操作（都是线程安全的）
counter.store(10);
int val = counter.load();
int prev = counter.exchange(20);         // 设置新值，返回旧值
counter.fetch_add(1);                    // 原子加
counter.fetch_sub(1);                    // 原子减
counter++;                               // 等价于 fetch_add(1)

// CAS (Compare-And-Swap)
int expected = 10;
bool success = counter.compare_exchange_strong(expected, 20);
// 如果 counter == expected，设置为 20，返回 true
// 否则 expected 被更新为 counter 当前值，返回 false

// ===== 内存序（高级话题）=====
// 从松到严：
// memory_order_relaxed     — 只保证原子性
// memory_order_acquire     — 读操作：后续读写不重排到此之前
// memory_order_release     — 写操作：之前读写不重排到此之后
// memory_order_acq_rel     — 同时 acquire + release
// memory_order_seq_cst     — 默认，最严格，全局一致顺序

// 大多数场景用默认的 seq_cst 即可
```

## 第九阶段：错误处理

### 9.1 异常

```
#include <stdexcept>

// ===== 抛出异常 =====
void validate(int age) {
    if (age < 0) {
        throw std::invalid_argument("Age cannot be negative");
    }
    if (age > 200) {
        throw std::out_of_range("Age too large");
    }
}

// ===== 捕获异常 =====
try {
    validate(age);
    riskyOperation();
} catch (const std::invalid_argument& e) {
    std::cerr << "Invalid: " << e.what() << "\n";
} catch (const std::exception& e) {
    std::cerr << "Error: " << e.what() << "\n";
} catch (...) {
    std::cerr << "Unknown error\n";
}

// ===== 标准异常层次 =====
// std::exception
// ├── std::logic_error
// │   ├── std::invalid_argument
// │   ├── std::out_of_range
// │   ├── std::domain_error
// │   └── std::length_error
// ├── std::runtime_error
// │   ├── std::overflow_error
// │   ├── std::underflow_error
// │   └── std::range_error
// ├── std::bad_alloc          (new 失败)
// ├── std::bad_cast           (dynamic_cast 失败)
// └── std::bad_variant_access (variant 访问错误类型)

// ===== 自定义异常 =====
class AppError : public std::runtime_error {
public:
    AppError(const std::string& msg, int code)
        : std::runtime_error(msg), code_(code) {}
    int code() const { return code_; }
private:
    int code_;
};

// ===== noexcept =====
void safe_func() noexcept { /* 保证不抛异常 */ }
// 移动构造/移动赋值/析构 应标记 noexcept
// swap 也应标记 noexcept
// 如果 noexcept 函数抛异常 → std::terminate

// ===== 异常安全保证 =====
// 1. 无保证     — 异常后状态未知（应避免）
// 2. 基本保证   — 异常后对象仍有效，但值可能改变
// 3. 强保证     — 异常后状态回滚到调用前（事务语义）
// 4. 不抛保证   — 绝不抛异常（noexcept）
```

### 9.2 无异常的错误处理

```
// ===== 方式 1: std::optional（值可能不存在）=====
std::optional<User> find_user(int id);

// ===== 方式 2: std::variant（成功或错误）=====
using Result = std::variant<Value, Error>;

// ===== 方式 3: 错误码 =====
std::error_code ec;
auto file = open_file("path", ec);
if (ec) {
    std::cerr << ec.message() << "\n";
}

// ===== 方式 4: std::expected (C++23) =====
// std::expected<Value, Error> — 最优雅，C++17 可用第三方库 tl::expected

// ===== 选择建议 =====
// 真正的异常情况（不可恢复）   → 异常
// 预期中的失败（查找失败等）   → optional / variant / error_code
// 性能敏感路径               → error_code / optional
```

## 第十阶段：工程实践

### 10.1 项目结构

```
project/
├── CMakeLists.txt          # 构建系统
├── README.md
├── .clang-format           # 代码格式化配置
├── .clang-tidy             # 静态分析配置
├── include/                # 公共头文件
│   └── project/
│       ├── module_a.h
│       └── module_b.h
├── src/                    # 源文件
│   ├── CMakeLists.txt
│   ├── module_a.cpp
│   └── module_b.cpp
├── tests/                  # 测试
│   ├── CMakeLists.txt
│   ├── test_module_a.cpp
│   └── test_module_b.cpp
├── examples/               # 示例
├── third_party/            # 第三方依赖
└── docs/                   # 文档
```

### 10.2 CMake 基础

```
cmake_minimum_required(VERSION 3.16)
project(MyProject VERSION 1.0 LANGUAGES CXX)

# C++17 标准
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# 库
add_library(mylib
    src/module_a.cpp
    src/module_b.cpp
)
target_include_directories(mylib PUBLIC include)
target_compile_options(mylib PRIVATE -Wall -Wextra -Wpedantic)

# 可执行文件
add_executable(myapp src/main.cpp)
target_link_libraries(myapp PRIVATE mylib)

# 测试
enable_testing()
add_executable(tests tests/test_module_a.cpp)
target_link_libraries(tests PRIVATE mylib)
add_test(NAME MyTests COMMAND tests)
```

### 10.3 编码规范速查

```
命名约定（Google C++ Style 参考）：
├── 类型名:     PascalCase       (MyClass, HttpRequest)
├── 函数名:     snake_case       (do_something) 或 camelCase
├── 变量名:     snake_case       (my_var)
├── 成员变量:   trailing_underscore_  (data_, name_)
├── 常量:       kPascalCase      (kMaxSize) 或 ALL_CAPS
├── 命名空间:   snake_case       (my_project)
├── 宏:         ALL_CAPS         (MAX_BUFFER_SIZE)  — 尽量避免宏
├── 模板参数:   PascalCase       (typename T, typename ValueType)
└── 文件名:     snake_case.cpp   (my_module.cpp)

核心规则：
✓ 每个头文件使用 #pragma once 或 include guard
✓ 头文件中不要 using namespace
✓ 优先使用 constexpr/const 而非 #define
✓ 优先使用 enum class 而非 enum
✓ 单一职责原则：一个类/函数只做一件事
✓ 函数不超过 40-60 行
✓ 每个文件不超过 500-1000 行
```

### 10.4 调试与工具

```
# 编译选项
g++ -std=c++17 -Wall -Wextra -Wpedantic -Werror  # 严格警告
g++ -g -O0                    # 调试构建
g++ -O2 -DNDEBUG              # 发布构建
g++ -fsanitize=address         # 地址消毒器（内存错误）
g++ -fsanitize=undefined       # 未定义行为消毒器
g++ -fsanitize=thread          # 线程消毒器（数据竞争）

# 静态分析
clang-tidy --checks='*' file.cpp
cppcheck --enable=all file.cpp

# 动态分析
valgrind --leak-check=full ./program   # 内存泄漏检测

# 性能分析
perf record ./program
perf report

# 在线工具
# https://godbolt.org  — Compiler Explorer（查看汇编）
# https://quick-bench.com — 性能基准测试
```

## 学习路线图总结

```
阶段 1-2: 语言基础 + OOP          ← 约 2-3 周
  │ 目标：能写出正确的类、继承、多态
  │
阶段 3:   RAII + 智能指针 + 移动语义  ← 约 1-2 周
  │ 目标：理解所有权模型，告别内存泄漏
  │
阶段 4:   STL 容器 + 算法          ← 约 2 周
  │ 目标：熟练使用 vector/map/算法，写出地道的 C++ 代码
  │
阶段 5:   模板编程                 ← 约 2 周
  │ 目标：能写泛型代码，理解 SFINAE/if constexpr
  │
阶段 6:   Lambda + 函数式          ← 约 1 周
  │ 目标：熟练使用 Lambda 配合 STL 算法
  │
阶段 7:   C++17 特性               ← 约 1-2 周
  │ 目标：掌握 optional/variant/string_view/filesystem 等
  │
阶段 8:   并发编程                 ← 约 2 周
  │ 目标：能写线程安全的程序
  │
阶段 9:   错误处理                 ← 约 1 周
  │ 目标：理解异常安全，选择正确的错误处理策略
  │
阶段 10:  工程实践                 ← 持续
    目标：CMake、代码规范、测试、调试
```
