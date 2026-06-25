# 🎯 Atmemarketing VOC Analysis Toolkit

> 亚马逊/电商用户评论 → NLP智能打标 → 交互式洞察报告

一套完整的 **Voice of Customer (VOC)** 分析工具，专为跨境电商卖家和营销团队设计。将原始用户评论转化为可行动的营销战略洞察。


---

## 📦 两个 Claude Code Skill

| Skill | 功能 | 输入 | 输出 |
|-------|------|------|------|
| 🏷️ **atmemarketing-voc-tagger** | NLP标签提取+情感分析 | Excel 评论数据 | 打标完成版.xlsx + JSON |
| 📊 **atmemarketing-voc-reporter** | HTML报告生成+营销战略 | 打标 JSON | 交互式 HTML 报告 |

### 18维标签体系

**10个产品维度**（品类相关）+ **8个行为场景维度**（品类通用）

| 产品维度 | 行为维度 |
|----------|----------|
| 核心功能、性能、设计、便携、电池、易用性、价格、可靠性、品牌、整体评价 | 🎯购买动因 📍使用地点 ⏰使用场景 👤用户画像 💡用户兴趣 🏷️品牌对比 🔮未满足需求 🔄复购忠诚 |

---

## 🚀 5分钟快速上手

### 1. 安装

```bash
# 克隆仓库
git clone https://github.com/atmemarketing/atmemarketing-voc.git
cd atmemarketing-voc

# 安装依赖
pip install pandas openpyxl

# 安装 Skill 到 Claude Code
cp -r skills/atmemarketing-voc-tagger ~/.claude/skills/
cp -r skills/atmemarketing-voc-reporter ~/.claude/skills/
```

### 2. 在 Claude Code 中使用

```
# 打标
/atmemarketing-voc-tagger
→ 提供你的 Excel 文件路径

# 生成报告
/atmemarketing-voc-reporter
→ 自动读取上一步的 JSON，生成 HTML 报告
```

### 3. 直接调用引擎

```python
from voc_engine import tag_and_export, build_report_html

# 一步打标
tags_data, xlsx_path = tag_and_export('评论数据.xlsx', tag_system, '产品名')

# 生成报告
html = build_report_html(tags_data, {'product_name': '产品名'})
with open('报告.html', 'w') as f: f.write(html)
```

---

## 📊 报告预览

生成的 HTML 报告包含 11 个板块：

1. 核心发现摘要
2. 标签全景散点图（ECharts 交互式）
3. 一级标签情感分布图
4. 一级标签散点图
5. 核心用户痛点深度解读
6. 产品核心优势
7. 目标用户画像
8. 核心使用场景
9. 产品改进建议
10. 附录：标签体系明细表
11. **营销战略方向总结**（社媒内容+网红合作+网站优化+精准投放）

---

## 📁 仓库结构

```
atmemarketing-voc/
├── README.md
├── voc_engine.py                    # 核心引擎（打标+报告）
├── skills/
│   ├── atmemarketing-voc-tagger/    # Skill 1: 打标
│   │   ├── SKILL.md
│   │   └── references/
│   └── atmemarketing-voc-reporter/  # Skill 2: 报告
│       ├── SKILL.md
│       └── references/
├── docs/
│   ├── QUICKSTART.md                # 快速上手
│   └── METHODOLOGY.md               # VOC分析方法论
└── examples/                        # 示例数据
```

---

## 🔧 技术要求

- Python 3.9+
- pandas, openpyxl
- ECharts（报告通过 CDN 加载，需网络）
- Claude Code（使用 Skill 时需要）

---

## 📄 License

MIT © Atmemarketing

---
## 🏢 关于 艾特密 / About @me

**艾特密（Atme Marketing）** 是一家专注跨境品牌增长的海外营销陪跑咨询机构。

我们不只出策略——更以 **"陪跑搭档"** 的角色，与品牌并肩走完从市场验证、策略制定到执行落地的每一步。

- 🕐 **10+ 年**跨境营销实战经验
- 🌍 覆盖 **20+ 国家**市场（北美、欧洲、东南亚、中东）
- 🏭 服务 **18+ 品牌**，覆盖 10+ 行业
- ⭐ **97%** 客户满意度
>
> **@me (Atme Marketing)** — Your cross-border brand growth partner. 10+ years experience across 20+ countries, 18+ brands served. We don't just strategize — we run the race with you.
>
>## 🛠️ 我们提供的服务

| 层级 | 服务 | 适合 |
|------|------|------|
| Step 1 | 海外销量机会情报包 | 正在评估海外机会的企业 |
| Step 2 | 1小时海外增长轻咨询 | 已出海但流量遇瓶颈的企业 |
| Step 3 | 海外增长实战训练营 | 需要系统提升团队能力的企业 |
| Step 4 | 基础陪跑 | 需要专业引导的品牌团队 |
| Step 5 | 深度陪跑 | 寻求全面增长体系搭建的品牌 |

>## 📮 找到我 / Get in Touch

- 💼 **公众号/视频号**：Anna跨境出海营销
- 📍 **地址**：深圳坂田
- 📱 **微信**：ianna666
- 📧 **邮箱**：[anna@atmemarketing.com](mailto:anna@atmemarketing.com)

*联系获取出海洞察 & 合作咨询*


> **全球视角起步，品牌战略落地**
> *"选择艾特密，不只是获得营销服务，更是找到一位共同成长的搭档，陪伴你从 0 到 1 搭建属于自己的海外增长引擎。"*
> *From Zero to One: Your Growth Partner for Global Markets*
> *Choosing @me isn't just hiring a marketing service — it's finding a partner who grows with you, helping you build your own overseas growth engine from scratch.*

## 🤝 贡献

欢迎提交 Issue 和 Pull Request。新品类标签体系贡献尤其欢迎！
