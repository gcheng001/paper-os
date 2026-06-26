---
name: paper-s7-ppt
description: '论文写作OS S7-答辩PPT制作。3环节模型+6部分框架+AI生成工作流。TRIGGER when: 用户输入"答辩PPT"或由paper-os总控调度。'
author: 高澄（微信cheng715）
version: 1.0
---

# S7 答辩PPT制作

## 工作定位

| 项 | 内容 |
|---|------|
| **前置条件** | S5/S6通过（可跳） |
| **门禁类型** | **硬门禁** — 大纲确认后才生成slides |
| **后置动作** | 更新 state.json（S7→pending_review）+ 追加 LOG.md |
| **de-ai-polish** | 不强制（PPT是要点式，非正文） |

---

## 执行流程

### Step 1：锁定客户（产品设计思维）

客户 = 答辩组老师（专业人士，认知负荷高）。

**删除一切基础科普内容**：
- ❌ "SWOT是指……它分别对……进行分析，能够……"——老师比你清楚
- ❌ "根据第××次全国互联网……报告的数据，我国已有网民……"——堆砌数据
- 这些内容老师听了很多遍，反复唠叨是一种冒犯
- ✅ 只保留老师们需要的重要信息

### Step 2：了解痛点

答辩委员的痛点：学生多、水平参差、PPT质量低、认知负荷重。

**最重要的痛点**：在恶劣环境下，老师还必须恪尽职守，为学生找到论文问题并给予评判和建议。

**解决方案**：
1. **严格控制文字量**（**每页≤20个汉字/英文单词**）——减轻认知负荷
2. **用美观正确的图表**——丑图或错误的图让印象大打折扣
3. **脱稿讲述 + 目光接触**——老师们会逐渐抬头看你，目光变得温和

**脱稿技巧**：
- 不要逐字念PPT，用自己的话讲清楚每页核心
- 做不到从容脱稿→事先写好稿子多读几遍，背不下来写小卡片当提词器
- ❌ 千万不要从头到尾低头念卡片

### Step 3：有效传递信号

用6部分框架（共6-7页）向老师有效传递"靠谱""优秀"的信号：

| 页 | 内容 | 要点 |
|----|------|------|
| 1. 问题 | 一句话说清研究问题 | 不用从句和被动句式，越简明越好 |
| 2. 价值 | 解决问题有什么用 | 不随意拔高，实事求是 |
| 3. 必要 | VOSviewer/CiteSpace文献分析图 | 说明研究处于哪个夹缝，别人没解决的 |
| 4. 设计 | 数据收集/处理/分析 | 简要说明 |
| 5. 讨论 | 核心部分 | 剖析违背常识的结果，给出解释 |
| 6. 限制 | 研究的不足 | 实事求是，有一说一 |

> 一共六七页PPT，寥寥数语把老师需要了解的重要信息全部汇报清楚。

### Step 3：生成PPT大纲（硬门禁）

PPT需突出展示6个部分（共6-7页）：

```markdown
# PPT大纲

## 第1页：问题
一句话说清楚研究问题，无从句无被动句

## 第2页：价值
实践作用或铺垫价值，不要夸大

## 第3页：必要性
VOSviewer/CiteSpace图 + 研究夹缝说明

## 第4页：设计
数据收集/处理/分析的简要说明

## 第5页：讨论
核心部分！违背常识的结果需剖析

## 第6页：限制
实事求是，体现尽力态度

## （可选）第7页：致谢
```

**→ 用户确认大纲后才进入生成**

### Step 4：生成slides

#### 出口一：Gamma导入（书中推荐）

1. 将大纲转换为 Markdown revealjs slide 格式
2. 导入 Gamma（Text transform模式）
3. 设置：简体中文 + 12张上限 + 自动配图

> revealjs格式示例见 `references/revealjs示例.md`

#### 出口二：frontend-slides（本地HTML）

调用 `frontend-slides` skill 生成 HTML 演示文稿。

#### 出口三：guizang-ppt-skill（网页PPT）

调用 `guizang-ppt-skill` 生成多风格HTML幻灯片。

---

## AI生成讲解内容提示词

```
请查询最新资料，用小学生也能听懂的语言，详细、生动地讲解以下内容：

[论文各部分的内容概要]
```

**转换为slides提示词**：
```
请将以下内容转换为 Markdown revealjs slide 格式。
要求：每段对应一页slide，每页不超过6项内容，数学公式用LaTeX。
```

---

## 输出约定

保存到：`outputs/S7-答辩PPT/`

```
outputs/S7-答辩PPT/
├── PPT大纲.md     # 硬门禁产物
└── slides.md       # revealjs格式slides
```

PPT大纲 frontmatter：
```yaml
---
step_id: S7
status: pending_review
key_conclusions:
  - PPT共X页
  - 核心页：讨论（第5页）
depends_on:
  - S5
  - S6
---
```

### 独立模式

用户提供论文或研究摘要，直接生成PPT大纲和slides，产出存桌面。

---

## 交接与评估

- **交接规范**：见 `~/.shared-skills/paper-os/references/handoff-protocol.md`（S7→S8 交接）
- **评估标准**：见 `~/.shared-skills/paper-os/references/evaluation-guide.md`（S7 评估维度）
- **产物 Schema**：见 `~/.shared-skills/paper-os/schema/handoff_package_schema.json`
