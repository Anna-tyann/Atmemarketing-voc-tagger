---
name: atmemarketing-voc-tagger
version: 2.0.0
description: >-
  Atmemarketing VOC 打标工具。将亚马逊/电商用户评论原始数据（Excel）通过 NLP 关键词匹配和情感分析，
  自动提取18维层级化标签（10产品维度+8行为场景维度），输出「打标完成版」Excel（原始数据+标签列+统计Sheet）
  和结构化 JSON。支持任意产品品类，行为维度品类通用自动合并。
metadata:
  author: Atmemarketing
  category: VOC分析
  requires:
    bins: ["python3"]
    libs: ["pandas", "openpyxl"]
  pipeline_stage: 1
  output: 打标完成版.xlsx + voc_tags_data.json
  homepage: https://github.com/atmemarketing/atmemarketing-voc
---

# Atmemarketing VOC 打标 Skill

## 概述

将原始亚马逊评论数据 → 18维结构化标签数据。

输入：Excel 文件（`.xlsx`，需含 `rid, title, content, rating` 列）
主输出：**「产品名_打标完成版.xlsx」** — 4 个 Sheet
辅助输出：`产品名_打标完成版.json` — 供报告生成使用

---

## 18维标签体系

### 产品功能维度（10个 — 品类相关，需配置）

1. 核心功能 2. 性能表现 3. 产品设计与做工 4. 便携与出行 5. 电池与续航
6. 易用性与操作 7. 价格与价值 8. 可靠性与售后 9. 品牌与推荐 10. 整体评价

### 行为场景维度（8个 — 品类通用，自动合并）

| 维度 | 用途 |
|------|------|
| 🎯 购买动因 | 转化文案、投放定向 |
| 📍 使用地点 | 场景化营销、网站设计 |
| ⏰ 使用场景 | 内容策略、使用时机 |
| 👤 用户画像 | 精准投放、人群定向 |
| 💡 用户兴趣/生活方式 | 网红合作、跨界营销 |
| 🏷️ 品牌/竞品对比 | 竞争策略、截流营销 |
| 🔮 未满足需求 | 产品创新、迭代方向 |
| 🔄 复购/忠诚行为 | 用户运营、CRM策略 |

---

## 执行流程

### Step 1: 确认输入文件和数据格式

检查 Excel 是否包含 `rid, title, content, rating` 列。如列名不同，先做映射。

### Step 2: 确认产品信息

向用户确认产品名称、品类。如果是新品类，需要构建专属的产品维度标签体系（行为维度自动通用）。

### Step 3: 构建标签体系

为新产品品类设计 L1/L2 标签和英文关键词。参考 `references/tag-system-guide.md`。

关键词设计原则：
- 多词短语优先（`"easy to use"` > `"easy"`）
- 英文关键词（匹配亚马逊英文评论）
- 10-20个关键词/L2
- 覆盖正负面表述

### Step 4: 执行打标

```python
import sys
# 引擎文件路径：优先当前目录，其次 Skill references
sys.path.insert(0, 'voc_engine.py所在目录')
from voc_engine import tag_and_export, get_default_tag_system

tag_system = get_default_tag_system()  # 或构建自定义标签体系
# 修改 tag_system 中的 L1/L2 名称和关键词以适配新品类

tags_data, output_path = tag_and_export(
    excel_path='数据文件.xlsx',
    tag_system=tag_system,
    product_name='产品名称',
)
```

### Step 5: 验证质量

正面率应与评分均值一致（偏差<20%）。每评论至少1个标签。痛点≥3个，优势≥3个。

### Step 6: 交付

输出 `产品名_打标完成版.xlsx`（4 Sheet）和 `产品名_打标完成版.json`。

---

## 引擎文件位置

`voc_engine.py` 查找顺序：
1. 当前工作目录
2. 本 Skill 的 `references/voc_engine.py`
3. GitHub 仓库根目录

---

## 注意事项

- 英文关键词，不要翻译为中文
- 数据隐私：所有处理在本地完成
- 大文件（>1000条）可能需要 1-2 分钟
- 标签体系是活的，第一次打标后可迭代优化
