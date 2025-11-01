# 📄 DOCX - Word 文档处理技能

## 概述

DOCX skill 提供全面的 Word 文档创建、编辑和分析能力，支持修订跟踪、注释、格式保留和文本提取。

**适用场景**：
- 创建新的 Word 文档
- 修改或编辑现有文档内容
- 处理修订跟踪（Track Changes）
- 添加注释和批注
- 提取文档内容和元数据

---

## 🎯 核心功能

### 1. 文档创建
使用 **docx-js** 库从零开始创建专业的 Word 文档。

**支持特性**：
- ✅ 段落和文本格式
- ✅ 表格和列表
- ✅ 页眉和页脚
- ✅ 图片和媒体
- ✅ 页面设置和样式

### 2. 文档编辑
使用 **Python OOXML 库**编辑现有 Word 文档。

**支持特性**：
- ✅ 文本替换和修改
- ✅ 格式保留
- ✅ 修订跟踪（Redlining）
- ✅ 注释管理
- ✅ 元数据修改

### 3. 文档分析
提取和分析 Word 文档内容。

**支持特性**：
- ✅ 文本提取（使用 Pandoc）
- ✅ 修订跟踪查看
- ✅ 注释提取
- ✅ 元数据读取
- ✅ 格式分析

---

## 🔧 使用方法

### 场景 1: 创建新文档

**示例需求**：
```
创建一份商业计划书，包含标题、多个章节、表格和格式化文本
```

**处理流程**：
1. 使用 docx-js 创建文档结构
2. 添加标题、段落和格式
3. 插入表格和列表
4. 导出为 .docx 文件

**关键技术**：
```javascript
const doc = new Document({
  sections: [{
    properties: {},
    children: [
      new Paragraph({
        text: "商业计划书",
        heading: HeadingLevel.HEADING_1
      }),
      new Paragraph({
        children: [
          new TextRun({
            text: "第一章：市场分析",
            bold: true
          })
        ]
      })
    ]
  }]
});
```

---

### 场景 2: 编辑现有文档（简单修改）

**示例需求**：
```
将文档中的"2024年"全部替换为"2025年"
```

**处理流程**：
1. 解包 DOCX 文件
2. 使用 Document 库读取和修改
3. 重新打包文档

**关键代码**：
```bash
# 解包
python ooxml/scripts/unpack.py input.docx unpacked/

# Python 脚本编辑
# ... Document library code ...

# 打包
python ooxml/scripts/pack.py unpacked/ output.docx
```

---

### 场景 3: 专业文档审阅（修订跟踪）

**示例需求**：
```
对法律合同进行专业审阅，所有修改需要显示修订跟踪
```

**处理流程（Redlining Workflow）**：

#### Step 1: 转换为 Markdown
```bash
pandoc --track-changes=all contract.docx -o current.md
```

#### Step 2: 识别修改点
在 Markdown 中标记所有需要修改的地方。

#### Step 3: 分批实施修改
将修改分组为 3-10 个一批，逐批实施：

```python
from ooxml.document import Document

# 加载文档
doc = Document("unpacked/word/document.xml")

# 批次 1: 修改第一段
batch_1 = [
  # 只标记实际修改的文字
  {
    "type": "replace",
    "old": "30",
    "new": "60",
    "context": "The term is 30 days"
  }
]

# 实施修改（使用 tracked changes）
for change in batch_1:
    doc.track_change(change)

doc.save()
```

#### Step 4: 验证修改
```bash
# 重新打包
python ooxml/scripts/pack.py unpacked/ reviewed.docx

# 转换查看
pandoc --track-changes=all reviewed.docx -o verify.md
```

**最佳实践**：
- ⚡ **最小化修改范围** - 只标记实际改变的文本
- 📦 **批量处理** - 3-10 个修改一批
- ✅ **逐批验证** - 每批完成后检查
- 🔒 **保留格式** - 保留原始 RSID

---

### 场景 4: 文档内容提取

**示例需求**：
```
从 Word 文档中提取所有文本和注释
```

**文本提取**：
```bash
# 转换为 Markdown
pandoc input.docx -o output.md

# 转换为纯文本
pandoc input.docx -o output.txt
```

**注释提取**：
```bash
# 解包文档
python ooxml/scripts/unpack.py input.docx unpacked/

# 读取注释文件
cat unpacked/word/comments.xml
```

---

## 💡 核心价值

### 📊 专业文档处理
- 保持文档格式和样式
- 支持复杂的文档结构
- 兼容 Microsoft Word

### 🔄 修订跟踪支持
- 专业的文档审阅流程
- 完整的修改历史
- 符合法律和商业规范

### 🚀 自动化能力
- 批量文档生成
- 模板化处理
- 程序化编辑

---

## 📚 实战案例

### 案例 1: 批量生成个性化合同

**需求**：
根据客户列表，批量生成个性化的服务合同。

**解决方案**：
```javascript
const generateContract = (customerData) => {
  const doc = new Document({
    sections: [{
      children: [
        new Paragraph({
          text: "服务合同",
          heading: HeadingLevel.HEADING_1
        }),
        new Paragraph({
          text: `甲方：${customerData.name}`
        }),
        new Paragraph({
          text: `地址：${customerData.address}`
        }),
        // ... 更多内容
      ]
    }]
  });

  Packer.toBuffer(doc).then(buffer => {
    fs.writeFileSync(`${customerData.id}_contract.docx`, buffer);
  });
};

// 批量生成
customers.forEach(generateContract);
```

---

### 案例 2: 法律文档专业审阅

**需求**：
对一份 50 页的法律合同进行审阅，所有修改需要显示修订跟踪。

**解决方案**：
1. **第一阶段：规划**
   - 转换为 Markdown 查看全文
   - 标记所有需要修改的条款
   - 分组为 10 个批次

2. **第二阶段：实施**
   - 批次 1-2: 修改定义条款（第 1-5 页）
   - 批次 3-5: 修改权利义务（第 6-20 页）
   - 批次 6-8: 修改违约条款（第 21-35 页）
   - 批次 9-10: 修改其他条款（第 36-50 页）

3. **第三阶段：验证**
   - 转换为 PDF 查看修订跟踪显示
   - 导出修订摘要
   - 提交客户审阅

**成果**：
- ✅ 所有修改完整记录
- ✅ 修改理由清晰可查
- ✅ 符合法律审阅规范

---

## ⚠️ 注意事项

### 最佳实践

**✅ 应该做的**：
- 创建新文档时使用 docx-js
- 编辑他人文档时使用修订跟踪
- 批量处理修改以便调试
- 保留原始文档格式
- 每批修改后验证结果

**❌ 不应该做的**：
- 不要跳过 Redlining 流程（专业文档）
- 不要一次修改太多内容
- 不要忽略 RSID 保留
- 不要在修订中包含未改变的文本
- 不要忘记验证公式错误

### 局限性

1. **复杂格式** - 极端复杂的格式可能需要手动调整
2. **嵌入对象** - 某些嵌入对象可能不完全支持
3. **宏代码** - 不支持 VBA 宏处理
4. **旧版格式** - .doc 格式需先转换为 .docx

---

## 🚀 快速开始

### 需求 1: "创建一份简历"

```
请帮我创建一份简历，包含个人信息、教育背景、工作经历和技能清单
```

Claude 会：
1. 使用 docx-js 创建文档
2. 添加结构化的简历内容
3. 应用专业格式
4. 导出为 .docx 文件

---

### 需求 2: "修改合同条款"

```
请将这份合同中的"30天"改为"60天"，并使用修订跟踪
```

Claude 会：
1. 解包文档
2. 定位需要修改的文本
3. 使用修订跟踪标记修改
4. 重新打包文档
5. 生成修订报告

---

### 需求 3: "提取文档摘要"

```
请提取这份 Word 文档的主要内容和所有注释
```

Claude 会：
1. 使用 Pandoc 提取文本
2. 解包读取注释 XML
3. 生成结构化摘要
4. 输出 Markdown 格式

---

## 📖 延伸阅读

### 完整技术文档
- `.claude/skills/docx/SKILL.md` - 完整技术规范
- `.claude/skills/docx/docx-js.md` - docx-js 库详细文档
- `.claude/skills/docx/ooxml.md` - OOXML 编辑完整指南

### 相关工具
- **Pandoc** - 文档格式转换
- **docx-js** - JavaScript Word 文档创建
- **python-docx** - Python Word 文档处理
- **Document Library** - OOXML 底层操作

---

## 🎓 使用提示

### 直接描述需求
```
帮我创建一份包含 3 个表格的报告
帮我修改这份合同的第5条款
提取这份文档的所有评论
```

### 提供具体要求
```
创建一份格式化的会议纪要，包括：
- 会议时间和地点
- 参会人员列表
- 讨论议题和决议
- 下一步行动计划
```

### 指定处理方式
```
使用修订跟踪模式修改这份法律文档
保持原有格式，只替换文字内容
生成 PDF 版本的修订对比
```

---

**记住：DOCX skill 是处理专业文档的强大工具，特别适合需要保持格式和修订跟踪的场景！** 📄✨
