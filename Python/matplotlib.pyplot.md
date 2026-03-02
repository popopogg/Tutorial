### 0. 基础设置 (必读)
在使用 Matplotlib 之前，通常需要运行以下代码以支持中文显示和负号显示：

```python
import matplotlib.pyplot as plt
import numpy as np

# 设置中文显示 (解决中文乱码问题)
plt.rcParams['font.sans-serif'] = ['SimHei']  # Windows用 SimHei，Mac用 Arial Unicode MS
plt.rcParams['axes.unicode_minus'] = False    # 解决负号显示为方块的问题
```

---

### 1. 画布与子图 (Canvas & Layout)

| 功能 | 代码示例 | 说明 |
| :--- | :--- | :--- |
| **创建画布** | `plt.figure(figsize=(10, 6), dpi=100)` | `figsize`: 宽,高(英寸); `dpi`: 分辨率 |
| **创建子图** | `fig, ax = plt.subplots(2, 2)` | 创建2行2列的子图，`ax`是子图对象数组 |
| **调整布局** | `plt.tight_layout()` | 自动调整子图间距，防止标签重叠 (强烈推荐) |
| **关闭画布** | `plt.close()` | 关闭当前窗口，释放内存 |

### 2. 常用图表类型 (Plot Types)

| 功能 | 代码示例 | 关键参数说明 |
| :--- | :--- | :--- |
| **折线图** | `plt.plot(x, y, 'r-o', label='图例名')` | `'r-o'`: 简写格式(红色-实线-圆点) |
| **散点图** | `plt.scatter(x, y, c='b', s=50, alpha=0.5)` | `c`: 颜色; `s`: 大小; `alpha`: 透明度(0-1) |
| **柱状图** | `plt.bar(x, height, width=0.8, color='g')` | 垂直柱状图。水平柱状图用 `plt.barh()` |
| **直方图** | `plt.hist(data, bins=30, density=True)` | `bins`: 箱子数量; `density`: 是否归一化 |
| **显示图片** | `plt.imshow(img, cmap='gray')` | `cmap`: 颜色映射 (如 gray, viridis, jet) |
| **饼图** | `plt.pie(sizes, labels=labels, autopct='%1.1f%%')` | `autopct`: 显示百分比格式 |
| **箱线图** | `plt.boxplot(data)` | 用于统计数据分布和离群值 |

### 3. 图表装饰与标注 (Decoration)

| 功能 | 代码示例 | 说明 |
| :--- | :--- | :--- |
| **标题** | `plt.title("总标题", fontsize=16)` | 可设置字体大小、颜色 |
| **轴标签** | `plt.xlabel("X轴名称")` / `plt.ylabel("...")` | 设置 X/Y 轴的含义 |
| **图例** | `plt.legend(loc='best')` | `loc`: 位置 (best, upper right, etc.) |
| **网格线** | `plt.grid(True, linestyle='--', alpha=0.5)` | 开启网格，设置线型和透明度 |
| **文本注释** | `plt.text(x, y, "文本内容")` | 在指定坐标 (x,y) 处添加文字 |
| **箭头标注** | `plt.annotate("重点", xy=(x,y), xytext=(x+1,y+1), arrowprops=dict(facecolor='black'))` | 带箭头的注释，指向 (xy) |

### 4. 坐标轴控制 (Axis Control)

| 功能 | 代码示例 | 说明 |
| :--- | :--- | :--- |
| **坐标范围** | `plt.xlim(0, 10)` / `plt.ylim(-5, 5)` | 强制设置显示范围 |
| **坐标刻度** | `plt.xticks([0, 5, 10], ['低', '中', '高'])` | 自定义刻度位置和对应的文字 |
| **对数坐标** | `plt.xscale('log')` / `plt.yscale('log')` | 将轴设置为对数刻度 |
| **轴状态** | `plt.axis('off')` | 隐藏坐标轴 (常用于显示图片) |
| **等比例** | `plt.axis('equal')` | 保证 x 轴和 y 轴刻度比例一致 (画圆时必备) |

### 5. 输出与显示 (Output)

| 功能 | 代码示例 | 说明 |
| :--- | :--- | :--- |
| **显示图表** | `plt.show()` | 渲染并弹出窗口 (Jupyter中通常不需要) |
| **保存图片** | `plt.savefig('name.png', dpi=300, bbox_inches='tight')` | `bbox_inches='tight'`: 去除周围多余白边 |

---

### 全流程代码模板 (Copy & Run)

```python
import matplotlib.pyplot as plt
import numpy as np

# 1. 数据准备
x = np.linspace(0, 10, 100)
y1 = np.sin(x)
y2 = np.cos(x)

# 2. 创建画布 (宽10, 高6)
plt.figure(figsize=(10, 6))

# 3. 绘图
plt.plot(x, y1, label='正弦波', color='blue', linewidth=2)
plt.plot(x, y2, label='余弦波', color='red', linestyle='--')

# 4. 装饰
plt.title("正弦与余弦函数示例")  # 需提前设置中文字体
plt.xlabel("时间 (s)")
plt.ylabel("幅值")
plt.legend(loc='upper right')    # 显示图例
plt.grid(True, alpha=0.3)        # 显示网格

# 5. 保存与显示
# plt.savefig('my_plot.png', dpi=300, bbox_inches='tight') # 保存要在show之前
plt.show()
```
