---
name: paper-s6-factcheck
description: '论文写作OS S6-AI事实核查。拆解事实声明+逐项核查+三级判定。TRIGGER when: 用户输入"论文事实核查""文献核查"或由paper-os总控调度。'
author: 高澄（微信cheng715）
version: 1.0
---

# S6 AI事实核查

## 工作定位

| 项 | 内容 |
|---|------|
| **前置条件** | S5通过（可跳） |
| **门禁类型** | **硬门禁** — 每条△存疑/×错误须用户逐条裁决 |
| **后置动作** | 更新 state.json（S6→pending_review）+ 追加 LOG.md |
| **de-ai-polish** | 不强制（本步是核查，不是写作） |

---

## 核心理念

**幽灵文献风险**：看起来完全合理但根本不存在的参考文献。期刊名、作者名、卷期号都写得逼真，肉眼无法识别。

**后果**：导师的批评可以忍受，但撤稿通知上的名字一旦印上就再也擦不掉。

**AI核查的局限**：AI核查不是百发百中（如"数模之星"赛制案例），×判定也要人工复核。但AI能指出的问题已经对质量提升帮助很大。

---

## 执行流程

### Step 1：拆解独立事实声明

将论文中的每个数据、日期、引用、因果断言拆解为独立的事实声明项。

### Step 2：核查通道分流

| 声明类型 | 核查通道 | 工具 |
|---------|---------|------|
| 法条引用 | 元典法条原文核对 | `mcp__yuandian-law__yuandian_rh_ft_detail`（核对原文+时效性） |
| 案例引用 | 元典案号核对 | `mcp__yuandian-case__yuandian_rh_case_details` 或 `rh_ptal_search`/`rh_qwal_search` |
| 法规引用 | 元典法规详情 | `mcp__yuandian-law__yuandian_rh_fg_detail` |
| 一般学术文献 | 网络验证 | WebSearch（Semantic Scholar/Google Scholar） |
| 数据/统计 | 原始数据源核对 | WebSearch + 官方数据库 |
| 日期/时间 | 原始来源核对 | WebSearch |

### Step 3：逐项核查（使用事实核查提示词）

> 完整提示词模板见 `references/事实核查提示词.md`

对每项使用以下格式输出：

```
【声明X】：[原文内容]
【核查结果】：√通过 / △存疑 / ×错误
【证据来源】：[具体的可查证来源]
【详细说明】：[判断依据，如有错误需提供正确信息]
```

### Step 4：总结统计

```
- 通过项：X 条
- 存疑项：X 条（需进一步验证）
- 错误项：X 条（已找到反证）
- 整体可信度评估：[基于证据的综合判断]
```

### Step 5：用户逐条裁决（硬门禁）

每条△/×项须用户做出裁决：
- △存疑 → 用户自行核实后标记 通过/错误
- ×错误 → 用户确认错误并修改论文 / 用户提供反证推翻判定

**注意事项**：
- AI核查也会误判（"数模之星"案例），×判定也要人工复核
- 中文文献AI核查能力较弱，可能因数据库不可达而标△
- 法条引用务必通过元典核对原文与时效性

---

## 输出约定

保存到：`outputs/S6-核查/事实核查报告.md`

产物 frontmatter：
```yaml
---
step_id: S6
status: pending_review
key_conclusions:
  - 通过X项，存疑Y项，错误Z项
  - 整体可信度：高/中/低
depends_on:
  - S5
---
```

### 独立模式

用户提供任意文稿（论文/报告/文书），直接执行事实核查，产出报告存桌面。

---

## 交接与评估

- **交接规范**：见 `~/.shared-skills/paper-os/references/handoff-protocol.md`（S6→S7 交接）
- **评估标准**：见 `~/.shared-skills/paper-os/references/evaluation-guide.md`（S6 评估维度 + 硬失败条件）
- **产物 Schema**：见 `~/.shared-skills/paper-os/schema/handoff_package_schema.json`
