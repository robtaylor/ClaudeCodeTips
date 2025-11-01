# 📗 XLSX - Excel 电子表格处理技能

## 概述

XLSX skill 提供全面的 Excel 电子表格处理能力，支持公式、格式化、数据分析和可视化。特别适合财务建模、数据分析和报表生成。

**适用场景**：
- 创建带公式和格式的电子表格
- 读取和分析数据
- 修改现有电子表格（保留公式）
- 数据分析和可视化
- 财务建模

---

## 🎯 核心功能

### 1. 数据分析
使用 pandas 进行强大的数据操作。

**支持特性**：
- ✅ 数据读取和写入
- ✅ 数据清洗和转换
- ✅ 统计分析
- ✅ 数据透视表

### 2. 电子表格创建
使用 openpyxl 或 xlsxwriter 创建专业电子表格。

**支持特性**：
- ✅ 公式和函数
- ✅ 格式化和样式
- ✅ 图表和可视化
- ✅ 条件格式

### 3. 财务建模
遵循行业标准的财务建模规范。

**支持特性**：
- ✅ 颜色编码（蓝色输入、黑色公式）
- ✅ 假设驱动模型
- ✅ 多场景分析
- ✅ 零公式错误

---

## 🔧 使用方法

### 场景 1: 数据分析

**示例需求**：
```
分析销售数据，计算每个产品的总销售额和平均价格
```

**使用 pandas**：
```python
import pandas as pd

# 读取数据
df = pd.read_excel('sales_data.xlsx')

# 分析
summary = df.groupby('product').agg({
    'quantity': 'sum',
    'price': 'mean',
    'revenue': 'sum'
})

# 输出
summary.to_excel('sales_summary.xlsx')
```

---

### 场景 2: 创建财务模型

**示例需求**：
```
创建一份3年财务预测模型，包括收入、成本和利润
```

**使用 openpyxl（关键代码）**：
```python
from openpyxl import Workbook
from openpyxl.styles import Font, PatternFill

wb = Workbook()
ws = wb.active
ws.title = "财务模型"

# 颜色定义
BLUE = Font(color="0000FF")  # 输入
BLACK = Font(color="000000")  # 公式
YELLOW_BG = PatternFill(start_color="FFFF00", fill_type="solid")  # 关键假设

# 假设区域
ws['A1'] = "关键假设"
ws['A1'].font = Font(bold=True)

ws['A2'] = "年增长率"
ws['B2'] = 0.15  # 15%
ws['B2'].font = BLUE
ws['B2'].fill = YELLOW_BG
ws['B2'].number_format = '0.0%'

# 收入预测（公式，黑色字体）
ws['A5'] = "年份"
ws['B5'] = 2025
ws['C5'] = 2026
ws['D5'] = 2027

ws['A6'] = "收入 ($mm)"
ws['B6'] = 100  # 基准年（蓝色）
ws['B6'].font = BLUE

# 公式（黑色）
ws['C6'] = '=B6*(1+$B$2)'
ws['D6'] = '=C6*(1+$B$2)'
ws['C6'].font = BLACK
ws['D6'].font = BLACK

# 格式化
for cell in [ws['B6'], ws['C6'], ws['D6']]:
    cell.number_format = '$#,##0;($#,##0);-'

wb.save('financial_model.xlsx')
```

---

### 场景 3: 批量数据处理

**示例需求**：
```
从50个Excel文件中提取特定数据并汇总
```

**使用 pandas**：
```python
import pandas as pd
import glob

# 读取所有Excel文件
all_files = glob.glob("data/*.xlsx")

# 合并数据
df_list = []
for file in all_files:
    df = pd.read_excel(file)
    df['source'] = file
    df_list.append(df)

# 汇总
combined_df = pd.concat(df_list, ignore_index=True)

# 统计
summary = combined_df.groupby('category').agg({
    'amount': ['sum', 'mean', 'count']
})

# 输出
combined_df.to_excel('combined_data.xlsx', index=False)
summary.to_excel('summary.xlsx')
```

---

### 场景 4: 添加图表

**示例需求**：
```
创建一个包含数据和柱状图的报表
```

**使用 openpyxl**：
```python
from openpyxl import Workbook
from openpyxl.chart import BarChart, Reference

wb = Workbook()
ws = wb.active

# 数据
data = [
    ['月份', '销售额'],
    ['1月', 1000],
    ['2月', 1200],
    ['3月', 1500],
    ['4月', 1300],
]

for row in data:
    ws.append(row)

# 创建图表
chart = BarChart()
chart.title = "月度销售额"
chart.x_axis.title = "月份"
chart.y_axis.title = "销售额"

# 数据范围
data_ref = Reference(ws, min_col=2, min_row=1, max_row=5)
cats_ref = Reference(ws, min_col=1, min_row=2, max_row=5)
chart.add_data(data_ref, titles_from_data=True)
chart.set_categories(cats_ref)

# 添加图表
ws.add_chart(chart, "D2")

wb.save('sales_chart.xlsx')
```

---

### 场景 5: 公式重算

**示例需求**：
```
修改Excel模型的假设值并重新计算所有公式
```

**使用 openpyxl + LibreOffice**：
```python
from openpyxl import load_workbook
import subprocess

# 修改假设
wb = load_workbook('model.xlsx')
ws = wb.active

ws['B2'] = 0.20  # 将增长率从15%改为20%

wb.save('model_updated.xlsx')

# 重算公式（使用LibreOffice）
subprocess.run([
    'python', 'ooxml/scripts/recalc.py',
    'model_updated.xlsx'
])
```

---

## 💡 财务建模最佳实践

### 颜色编码标准

| 颜色 | 用途 | RGB |
|------|------|-----|
| 蓝色字体 | 硬编码输入 | (0, 0, 255) |
| 黑色字体 | 公式计算 | (0, 0, 0) |
| 绿色字体 | 工作表内链接 | (0, 128, 0) |
| 红色字体 | 外部文件链接 | (255, 0, 0) |
| 黄色背景 | 关键假设 | (255, 255, 0) |

### 数字格式标准

```python
# 年份：文本格式
ws['B5'] = "2024"

# 货币：千分位，单位在标题
ws['A6'] = "收入 ($mm)"
ws['B6'].number_format = '$#,##0;($#,##0);-'

# 百分比：一位小数
ws['B7'].number_format = '0.0%'

# 倍数：一位小数
ws['B8'].number_format = '0.0x'

# 负数：括号显示
# 123 vs (123)
```

### 公式构建规则

**✅ 正确做法**：
```python
# 使用单元格引用，不要硬编码
ws['C6'] = '=B6*(1+$B$2)'  # 好

# 不要这样
ws['C6'] = '=B6*1.15'  # 差
```

**文档化硬编码值**：
```python
ws['B2'] = 0.15
ws['C2'] = "来源：公司财报 FY2024, Page 12"
```

---

## 📚 实战案例

### 案例 1: DCF 估值模型

**需求**：
创建完整的 DCF（折现现金流）估值模型。

**解决方案结构**：

```
工作表：
1. 假设
   - 收入增长率（蓝色输入）
   - 营业利润率（蓝色输入）
   - 资本开支率（蓝色输入）
   - 折现率（WACC）（蓝色输入）
   - 永续增长率（蓝色输入）

2. 损益表
   - 收入预测（公式，黑色）
   - 营业成本（公式）
   - 营业利润（公式）

3. 现金流
   - EBIT
   - 税后调整
   - 折旧摊销
   - 资本开支
   - 营运资金变化
   - 自由现金流（FCF）

4. 估值
   - FCF 折现
   - 终值计算
   - 企业价值（EV）
   - 每股价值
```

**关键公式示例**：
```python
# 收入预测（引用假设）
ws['C10'] = '=B10*(1+假设!$B$2)'

# 自由现金流
ws['C20'] = '=C15-C16-C17-C18'  # NOPAT-CapEx-WC Change

# 折现因子
ws['C22'] = '=1/((1+假设!$B$8)^C5)'  # 1/(1+WACC)^year

# 折现后FCF
ws['C23'] = '=C20*C22'
```

---

### 案例 2: 数据仪表板

**需求**：
创建交互式销售数据仪表板。

**功能**：
- 多个数据透视表
- 动态图表
- 条件格式化
- 切片器和筛选器

**实现**：
```python
import pandas as pd
from openpyxl import Workbook
from openpyxl.chart import LineChart, Reference
from openpyxl.styles import PatternFill

# 读取原始数据
df = pd.read_excel('sales_raw.xlsx')

# 创建透视表
pivot = pd.pivot_table(
    df,
    values='amount',
    index='month',
    columns='product',
    aggfunc='sum'
)

# 写入 Excel
wb = Workbook()
ws_data = wb.active
ws_data.title = "原始数据"

# 透视表工作表
ws_pivot = wb.create_sheet("汇总")
# ... 写入透视数据 ...

# 添加条件格式
from openpyxl.formatting.rule import ColorScaleRule
rule = ColorScaleRule(
    start_type='min', start_color='FF0000',
    end_type='max', end_color='00FF00'
)
ws_pivot.conditional_formatting.add('B2:E13', rule)

wb.save('dashboard.xlsx')
```

---

## ⚠️ 注意事项

### 最佳实践

**✅ 应该做的**：
- 使用公式而非硬编码计算值
- 遵循颜色编码标准
- 将假设集中管理
- 测试边界情况（零值、负值）
- 验证公式无错误
- 添加数据来源文档

**❌ 不应该做的**：
- 在公式中硬编码数值
- 混淆输入和公式的颜色
- 隐藏重要假设
- 忽略公式错误
- 使用不一致的格式

### 零公式错误要求

**强制要求**：交付的 Excel 文件必须零公式错误。

检查项：
- ❌ #REF! - 引用错误
- ❌ #DIV/0! - 除零错误
- ❌ #VALUE! - 值错误
- ❌ #N/A - 不可用
- ❌ #NAME? - 名称错误

**解决方案**：
```python
# 使用 IFERROR 保护公式
ws['C6'] = '=IFERROR(B6/B5, "-")'

# 检查零值
ws['C7'] = '=IF(B5=0, "-", B6/B5)'
```

---

## 🚀 快速开始

### 需求 1: "分析销售数据"

```
分析这份销售数据，找出销售额最高的产品和地区
```

Claude 会：
1. 使用 pandas 读取数据
2. 进行分组和聚合分析
3. 生成汇总报表
4. 创建可视化图表

---

### 需求 2: "创建财务模型"

```
创建一个5年收入预测模型，年增长率15%
```

Claude 会：
1. 创建假设区域（蓝色输入）
2. 建立收入公式（黑色公式）
3. 应用正确的颜色编码
4. 格式化数字显示
5. 添加数据来源说明

---

### 需求 3: "合并多个文件"

```
将 sales 文件夹下的所有 Excel 文件合并为一个文件
```

Claude 会：
1. 扫描文件夹
2. 读取所有 Excel 文件
3. 合并数据
4. 添加来源标识
5. 输出合并文件

---

## 📖 延伸阅读

### 完整技术文档
- `.claude/skills/xlsx/SKILL.md` - 完整技术规范
- `.claude/skills/xlsx/reference.md` - API 详细参考

### 相关工具
- **pandas** - 数据分析
- **openpyxl** - Excel 文件操作
- **xlsxwriter** - Excel 文件生成
- **xlwings** - Excel 自动化

### 财务建模资源
- Wall Street Prep 建模指南
- Macabacus 最佳实践
- CFA Institute 标准

---

**记住：XLSX skill 是数据分析和财务建模的强大工具，遵循行业标准，确保专业性！** 📗✨
