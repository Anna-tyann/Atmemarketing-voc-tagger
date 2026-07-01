---
name: atmemarketing-voc-tagger
version: 2.1.0
description: >-
  Atmemarketing VOC 打标工具。将亚马逊/电商用户评论原始数据（Excel）通过 NLP 关键词匹配和情感分析，
  自动提取22维层级化标签（10产品维度+12行为场景维度），输出「打标完成版」Excel（原始数据+标签列+统计Sheet）
  和结构化 JSON（含交叉分析矩阵、词云数据）。**品类通用**，支持任意产品。
metadata:
  author: Atmemarketing
  category: VOC分析
  requires:
    bins: ["python3"]
    libs: ["pandas", "openpyxl"]
  pipeline_stage: 1
  output: 打标完成版.xlsx + voc_tags_data.json (含交叉分析+词云)
  homepage: https://github.com/Anna-tyann/Atmemarketing-voc-tagger
---

# Atmemarketing VOC 打标 Skill

## 概述

将原始评论数据 → 22维结构化标签数据（含交叉分析矩阵、高频词云）。

**输入**：Excel 文件（`.xlsx`，需含 `rid, title, content, rating` 列）
**主输出**：`产品名_打标完成版.xlsx` — 4 个 Sheet（打标数据 / L2标签统计 / 用户痛点 / 产品优势）
**辅助输出**：`产品名_打标完成版.json` — 含标签明细、交叉分析矩阵、词云数据

## 核心特性（v2.1）

- ✅ **品类通用** — 22维标签体系适配冲牙器 / 电动自行车 / 猫抓板 / 手机壳等任意品类
- ✅ **数据驱动画像** — 用户画像和场景直接从真实打标标签提取，不再硬编码
- ✅ **交叉分析矩阵** — 自动生成 5 个维度交叉矩阵（人群×痛点、场景×痛点等）
- ✅ **词云数据** — 从标签关键词提取 Top 80 高频词
- ✅ **自适应气泡** — 散点图气泡大小根据样本量自动调整（大样本自动缩小）

---

## 22维标签体系（品类通用）

### 产品功能维度（10个 — 通用框架）

1. 核心功能与效果 2. 性能表现 3. 设计与做工 4. 便携性 5. 电池与续航
6. 易用性 7. 价格与价值 8. 可靠性与售后 9. 品牌与推荐度 10. 整体评价

### 行为场景维度（12个 — 品类通用，自动合并）

| 维度 | 用途 |
|------|------|
| 🎯 购买动因 | 转化文案、投放定向 |
| 📍 使用地点 | 场景化营销、网站设计 |
| ⏰ 使用场景 | 内容策略、使用时机 |
| 👤 用户画像 | 精准投放、人群定向 |
| 💡 用户兴趣/生活方式 | 网红合作、跨界营销 |
| 🏷️ 品牌/竞品对比 | 竞争策略、截流营销 |
| 🔮 未满足需求/期望 | 产品创新、迭代方向 |
| 🔄 复购/忠诚行为 | 用户运营、CRM策略 |
| 🛤️ 旅程阶段 | 用户生命周期管理 |
| 📢 声音类型 | UGC素材挖掘、口碑管理 |
| 💥 实际后果/影响 | ROI验证、价值主张 |
| 🧠 心理/情感类型 | 情感hook、文案策略 |

---

## 执行流程

### Step 1: 确认输入文件和数据格式

检查 Excel 是否包含 `rid, title, content, rating` 列。如列名不同，先做映射。

### Step 2: 确认产品信息

向用户确认产品名称、品类。标签体系已品类通用，无需手动构建。

### Step 3: 执行打标

```python
import sys
sys.path.insert(0, 'voc_engine.py所在目录')
from voc_engine import tag_and_export, get_default_tag_system

tag_system = get_default_tag_system()  # 品类通用标签体系

tags_data, output_path = tag_and_export(
    excel_path='数据文件.xlsx',
    tag_system=tag_system,
    product_name='产品名称',
)
```

### Step 4: 验证质量

正面率应与评分均值一致（偏差<20%）。每评论至少1个标签。痛点≥3个，优势≥3个。

### Step 5: 交付

输出 `产品名_打标完成版.xlsx`（4 Sheet）和 `产品名_打标完成版.json`（含交叉分析+词云）。

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
- JSON 输出含 `cross_analysis` 和 `wordcloud` 字段，可直接供 reporter 使用
