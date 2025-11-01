# 📕 PDF - PDF 文档处理技能

## 概述

PDF skill 提供全面的 PDF 处理能力，包括文本和表格提取、PDF创建、合并/拆分文档、以及表单填写。

**适用场景**：
- 提取 PDF 文本和表格数据
- 创建新的 PDF 文档
- 合并或拆分 PDF 文件
- 填写 PDF 表单
- 添加水印和注释
- PDF 元数据操作

---

## 🎯 核心功能

### 1. 文本和表格提取
从 PDF 中提取结构化内容。

**支持特性**：
- ✅ 文本提取（保留布局）
- ✅ 表格识别和提取
- ✅ 多页处理
- ✅ OCR 支持（扫描PDF）

### 2. PDF 创建和修改
生成或修改 PDF 文档。

**支持特性**：
- ✅ 从头创建 PDF
- ✅ HTML 转 PDF
- ✅ 图片转 PDF
- ✅ 添加水印

### 3. PDF 操作
合并、拆分和重组PDF。

**支持特性**：
- ✅ 合并多个PDF
- ✅ 拆分PDF为单页
- ✅ 页面旋转
- ✅ 页面提取

### 4. 表单处理
填写和提取 PDF 表单。

**支持特性**：
- ✅ 读取表单字段
- ✅ 填写表单数据
- ✅ 扁平化表单
- ✅ 批量处理

---

## 🔧 使用方法

### 场景 1: 提取 PDF 文本

**示例需求**：
```
从这份PDF报告中提取所有文本内容
```

**使用 pypdf**：
```python
from pypdf import PdfReader

reader = PdfReader("report.pdf")
text = ""
for page in reader.pages:
    text += page.extract_text()

print(text)
```

---

### 场景 2: 提取 PDF 表格

**示例需求**：
```
从财务报表PDF中提取所有表格数据
```

**使用 pdfplumber**：
```python
import pdfplumber

with pdfplumber.open("financial_report.pdf") as pdf:
    for page_num, page in enumerate(pdf.pages, 1):
        tables = page.extract_tables()
        for table_num, table in enumerate(tables, 1):
            print(f"Page {page_num}, Table {table_num}:")
            for row in table:
                print(row)
```

**导出为 CSV**：
```python
import pandas as pd

with pdfplumber.open("report.pdf") as pdf:
    page = pdf.pages[0]
    table = page.extract_table()
    df = pd.DataFrame(table[1:], columns=table[0])
    df.to_csv("extracted_table.csv", index=False)
```

---

### 场景 3: 合并 PDF 文件

**示例需求**：
```
将3份PDF报告合并为一份完整文档
```

**使用 pypdf**：
```python
from pypdf import PdfWriter, PdfReader

writer = PdfWriter()

# 添加所有PDF
for pdf_file in ["report1.pdf", "report2.pdf", "report3.pdf"]:
    reader = PdfReader(pdf_file)
    for page in reader.pages:
        writer.add_page(page)

# 保存合并后的文档
with open("merged_report.pdf", "wb") as output:
    writer.write(output)
```

---

### 场景 4: 拆分 PDF

**示例需求**：
```
将一份50页的PDF拆分为单独的页面
```

**使用 pypdf**：
```python
from pypdf import PdfReader, PdfWriter

reader = PdfReader("large_document.pdf")

for i, page in enumerate(reader.pages):
    writer = PdfWriter()
    writer.add_page(page)

    with open(f"page_{i+1}.pdf", "wb") as output:
        writer.write(output)
```

---

### 场景 5: 填写 PDF 表单

**示例需求**：
```
批量填写PDF申请表，每个员工一份
```

**读取表单字段**：
```python
from pypdf import PdfReader

reader = PdfReader("application_form.pdf")
fields = reader.get_fields()

for field_name, field in fields.items():
    print(f"{field_name}: {field.get('/V', '')}")
```

**填写表单**：
```python
from pypdf import PdfWriter, PdfReader

reader = PdfReader("application_form.pdf")
writer = PdfWriter()

# 复制页面
writer.append_pages_from_reader(reader)

# 填写字段
writer.update_page_form_field_values(
    writer.pages[0],
    {
        "name": "张三",
        "department": "技术部",
        "date": "2025-11-01"
    }
)

# 保存
with open("filled_form.pdf", "wb") as output:
    writer.write(output)
```

---

### 场景 6: 添加水印

**示例需求**：
```
为所有PDF页面添加"机密"水印
```

**使用 pypdf**：
```python
from pypdf import PdfReader, PdfWriter
from reportlab.pdfgen import canvas
from reportlab.lib.pagesizes import letter
import io

# 创建水印
def create_watermark(text):
    packet = io.BytesIO()
    can = canvas.Canvas(packet, pagesize=letter)
    can.setFont("Helvetica", 60)
    can.setFillColorRGB(0.5, 0.5, 0.5, alpha=0.3)
    can.drawString(200, 400, text)
    can.save()
    packet.seek(0)
    return PdfReader(packet)

# 应用水印
watermark = create_watermark("机密")
reader = PdfReader("document.pdf")
writer = PdfWriter()

for page in reader.pages:
    page.merge_page(watermark.pages[0])
    writer.add_page(page)

with open("watermarked.pdf", "wb") as output:
    writer.write(output)
```

---

## 💡 核心价值

### 📊 数据提取能力
- 从 PDF 中提取结构化数据
- 支持表格识别
- OCR 扫描文档处理

### 🔄 文档操作灵活性
- 合并、拆分、重组
- 页面级别的精确控制
- 批量处理支持

### 📝 表单自动化
- 自动填写 PDF 表单
- 批量数据导入
- 表单扁平化

---

## 📚 实战案例

### 案例 1: 批量提取发票数据

**需求**：
从 100 份 PDF 发票中提取关键信息（发票号、金额、日期）。

**解决方案**：
```python
import pdfplumber
import pandas as pd
import re

def extract_invoice_data(pdf_path):
    data = {
        "invoice_number": None,
        "amount": None,
        "date": None
    }

    with pdfplumber.open(pdf_path) as pdf:
        text = pdf.pages[0].extract_text()

        # 提取发票号
        invoice_match = re.search(r'发票号[：:]\s*(\S+)', text)
        if invoice_match:
            data["invoice_number"] = invoice_match.group(1)

        # 提取金额
        amount_match = re.search(r'金额[：:]\s*¥?([\d,]+\.?\d*)', text)
        if amount_match:
            data["amount"] = amount_match.group(1)

        # 提取日期
        date_match = re.search(r'日期[：:]\s*(\d{4}[-/]\d{2}[-/]\d{2})', text)
        if date_match:
            data["date"] = date_match.group(1)

    return data

# 批量处理
invoices = []
for i in range(1, 101):
    invoice_data = extract_invoice_data(f"invoice_{i}.pdf")
    invoices.append(invoice_data)

# 导出为 Excel
df = pd.DataFrame(invoices)
df.to_excel("invoices_summary.xlsx", index=False)
```

---

### 案例 2: 合同文档自动化

**需求**：
1. 合并多份合同附件
2. 添加页码
3. 添加机密水印
4. 生成目录

**解决方案**：
```python
from pypdf import PdfWriter, PdfReader
from reportlab.pdfgen import canvas
import io

def create_toc(contracts):
    """创建目录页"""
    packet = io.BytesIO()
    can = canvas.Canvas(packet)
    can.setFont("Helvetica-Bold", 16)
    can.drawString(50, 750, "合同目录")

    y = 700
    for i, contract_name in enumerate(contracts, 1):
        can.setFont("Helvetica", 12)
        can.drawString(50, y, f"{i}. {contract_name}")
        y -= 30

    can.save()
    packet.seek(0)
    return PdfReader(packet)

# 合并处理
contracts = {
    "主合同.pdf": "主合同",
    "附件A.pdf": "技术规格说明",
    "附件B.pdf": "报价单"
}

writer = PdfWriter()

# 添加目录
toc = create_toc(contracts.values())
writer.add_page(toc.pages[0])

# 添加合同
for pdf_file in contracts.keys():
    reader = PdfReader(pdf_file)
    for page in reader.pages:
        writer.add_page(page)

# 保存
with open("合同汇编.pdf", "wb") as output:
    writer.write(output)
```

---

## ⚠️ 注意事项

### 最佳实践

**✅ 应该做的**：
- 处理前检查 PDF 是否加密
- 使用适合的库（pypdf vs pdfplumber）
- 验证提取的数据准确性
- 处理异常和边界情况
- 批量操作前先测试单个文件

**❌ 不应该做的**：
- 不要假设所有 PDF 都是文本型
- 不要忽略 PDF 版本兼容性
- 不要在未验证的情况下批量处理
- 不要忽略内存限制（大文件）

### 局限性

1. **扫描 PDF** - 需要 OCR 才能提取文本
2. **复杂布局** - 多栏布局可能提取困难
3. **图片型表格** - 无法直接提取数据
4. **加密 PDF** - 需要密码才能处理
5. **特殊字体** - 某些字体可能无法正确提取

---

## 🚀 快速开始

### 需求 1: "提取 PDF 表格"

```
从这份财务报表 PDF 中提取所有表格数据
```

Claude 会：
1. 使用 pdfplumber 打开 PDF
2. 识别表格结构
3. 提取表格数据
4. 导出为 CSV/Excel

---

### 需求 2: "合并 PDF"

```
将这3份报告合并为一份完整的PDF
```

Claude 会：
1. 使用 pypdf 读取所有 PDF
2. 按顺序合并页面
3. 保存为新的 PDF 文件

---

### 需求 3: "批量填写表单"

```
根据 Excel 数据批量填写 PDF 申请表
```

Claude 会：
1. 读取 Excel 数据
2. 遍历每行数据
3. 填写 PDF 表单字段
4. 生成多份个性化 PDF

---

## 📖 延伸阅读

### 完整技术文档
- `.claude/skills/pdf/SKILL.md` - 完整技术规范
- `.claude/skills/pdf/reference.md` - 详细API参考
- `.claude/skills/pdf/forms.md` - 表单处理指南

### 相关工具
- **pypdf** - PDF 基础操作
- **pdfplumber** - 文本和表格提取
- **pdf2image** - PDF 转图片
- **OCRmyPDF** - 为扫描PDF添加文本层

---

**记住：PDF skill 是处理 PDF 文档的瑞士军刀，从简单提取到复杂操作都能胜任！** 📕✨
