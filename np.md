NumPy 是 Python 数据科学的核心库，主要用于**高性能的矩阵计算**。它的核心是 `ndarray`（N-dimensional array，多维数组）。

以下是 NumPy 的**快速入门指南**，涵盖了 90% 的日常使用场景。

### 0. 引入库
约定俗成的写法：
```python
import numpy as np
```

---

### 1. 创建数组 (Creation)
NumPy 数组比 Python 列表（List）更紧凑、更快。

```python
# 从列表创建
arr = np.array([1, 2, 3])           # 一维数组
mat = np.array([[1, 2], [3, 4]])    # 二维数组 (矩阵)

# 快速生成
nums = np.arange(0, 10, 2)          # 生成 [0, 2, 4, 6, 8] (类似 range)
line = np.linspace(0, 10, 5)        # 0到10之间均匀取5个点 (含头尾)

# 特殊矩阵
zeros = np.zeros((3, 3))            # 3x3 全0矩阵
ones  = np.ones((2, 5))             # 2x5 全1矩阵
eye   = np.eye(4)                   # 4x4 单位矩阵 (对角线为1)

# 随机数
rand_u = np.random.rand(3, 3)       # 0到1之间的均匀分布
rand_n = np.random.randn(3, 3)      # 标准正态分布 (均值0, 方差1)
rand_i = np.random.randint(0, 10, size=(2,2)) # 0-10之间的随机整数
```

---

### 2. 数组属性 (Attributes)
了解你正在处理的数据长什么样。

```python
arr = np.zeros((5, 3))

print(arr.ndim)   # 维度数 -> 2
print(arr.shape)  # 形状 (行, 列) -> (5, 3)
print(arr.size)   # 元素总数 -> 15
print(arr.dtype)  # 数据类型 -> float64
```

---

### 3. 索引与切片 (Indexing & Slicing)
操作方式类似 Python 列表，但支持多维操作。

```python
a = np.array([[1, 2, 3], 
              [4, 5, 6], 
              [7, 8, 9]])

# 基础索引 [行, 列]
print(a[1, 2])       # 取第2行第3列 -> 6

# 切片 [起始:结束:步长]
print(a[0, :])       # 取第1行所有元素 -> [1, 2, 3]
print(a[:, 1])       # 取第2列所有元素 -> [2, 5, 8]
print(a[0:2, 1:3])   # 取前两行，第2-3列 -> [[2, 3], [5, 6]]

# 🔥 布尔索引 (非常常用：用于筛选数据)
print(a[a > 5])      # 筛选出所有大于5的数 -> [6 7 8 9]
```

---

### 4. 数学运算 (Operations)
NumPy 的强大之处在于**广播机制 (Broadcasting)**，可以直接对整个数组进行运算，无需写 `for` 循环。

```python
x = np.array([1, 2, 3])
y = np.array([4, 5, 6])

# 元素级运算 (Element-wise)
print(x + y)      # [5 7 9]
print(x * 10)     # [10 20 30]
print(x ** 2)     # [1 4 9]

# 矩阵乘法 (点积)
# 1x3 乘 3x1
print(x.dot(y))   # 1*4 + 2*5 + 3*6 = 32
# 或者使用 @ 符号 (Python 3.5+)
print(x @ y)      # 32
```

---

### 5. 常用统计函数 (Statistics)
通常可以指定 `axis` 参数：`axis=0` 代表按列计算（垂直），`axis=1` 代表按行计算（水平）。

```python
arr = np.array([[1, 2, 3], 
                [4, 5, 6]])

print(arr.sum())          # 所有元素之和 -> 21
print(arr.mean())         # 平均值 -> 3.5
print(arr.max())          # 最大值 -> 6
print(arr.argmax())       # 最大值的索引位置

# 指定维度
print(arr.sum(axis=0))    # 按列求和 -> [5 7 9]
print(arr.mean(axis=1))   # 按行求平均 -> [2. 5.]
```

---

### 6. 形状变换 (Reshape)
改变数据的排列方式，但不改变数据本身。

```python
arr = np.arange(12)       # [0, 1, ..., 11]

# 改变形状
new_arr = arr.reshape(3, 4)  # 变身 3行4列
# new_arr = arr.reshape(3, -1) # -1 表示自动计算该维度，效果同上

# 展平
flat = new_arr.flatten()     # 变回一维

# 转置 (行列互换)
t_arr = new_arr.T
```

---

### 总结：NumPy 速查表

| 功能 | 代码 | 备注 |
| :--- | :--- | :--- |
| **生成序列** | `np.arange(start, end, step)` | 不含 end |
| **生成等分点** | `np.linspace(start, end, num)` | 含 end |
| **随机小数** | `np.random.rand(d0, d1)` | 0~1之间 |
| **随机整数** | `np.random.randint(low, high, size)` | |
| **查看形状** | `arr.shape` | 返回元组 (行, 列) |
| **改变形状** | `arr.reshape(m, n)` | 元素总数必须一致 |
| **筛选数据** | `arr[arr > val]` | 布尔索引 |
| **矩阵乘法** | `np.dot(a, b)` 或 `a @ b` | 线性代数核心 |
| **合并** | `np.vstack([a, b])` / `np.hstack([a, b])` | 垂直/水平堆叠 |
| **保存/读取** | `np.save('x.npy', a)` / `np.load('x.npy')` | 二进制快速存取 |
