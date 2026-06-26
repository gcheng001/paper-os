# paper-os · 论文写作操作系统

> 一套面向中文研究者的 Claude Code Agent Skills，把学位论文/期刊论文写作拆成 8 个可独立调度的阶段，按 **选题 → 文献 → 写作 → 质检 → 答辩** 主线推进，支持断点续办。

作者：**高澄（微信 cheng715）**

---

## 设计理念

写论文不是一次性产出，而是一条长链条的工程。`paper-os` 把这条链拆成 8 个独立 Skill，由总控统一调度，每个阶段都有自己的产物清单和交付门禁。你可以从头跑到尾，也可以只挑某一阶段用。

- **模块化**：总控 + 8 个子技能，各自独立，统一调度
- **可断点**：每个阶段产出落盘，随时中断、随时续办
- **带质检**：内置论文自检清单 + 事实核查 / 幻觉校验环节
- **覆盖答辩**：从选题一直管到答辩 PPT 和答辩准备

## 技能结构

| 技能 | 目录 | 阶段 | 作用 |
|------|------|------|------|
| paper-os | [`paper-os/`](paper-os/) | 总控 | 统一入口与调度器，主线串联 8 个阶段 |
| S1 | [`paper-s1-topic/`](paper-s1-topic/) | 选题 | 选题导向 → 细化 → 验证 → AI 辅助 → 导师沟通 |
| S2 | [`paper-s2-literature/`](paper-s2-literature/) | 文献 | 文献检索、文献矩阵、文献综述写作 |
| S3 | [`paper-s3-writing/`](paper-s3-writing/) | 写作 | 正文写作 |
| S4 | [`paper-s4-discussion/`](paper-s4-discussion/) | 讨论 | 讨论部分撰写 |
| S5 | [`paper-s5-checklist/`](paper-s5-checklist/) | 质检 | 论文自检清单 |
| S6 | [`paper-s6-factcheck/`](paper-s6-factcheck/) | 核查 | 事实核查 / 幻觉校验 |
| S7 | [`paper-s7-ppt/`](paper-s7-ppt/) | 答辩 | 答辩 PPT |
| S8 | [`paper-s8-defense/`](paper-s8-defense/) | 答辩 | 答辩准备 |

每个子目录下的 `SKILL.md` 是该技能的核心流程文件，部分技能还附带 `references/`（参考资料）、`templates/`（模板）、`examples/`（示例）、`schema/`（状态结构）。

## 安装

把对应目录复制到 Claude Code 的 skills 目录即可：

```bash
# 复制全部 9 个技能
cp -R paper-os paper-s1-topic paper-s2-literature paper-s3-writing \
         paper-s4-discussion paper-s5-checklist paper-s6-factcheck \
         paper-s7-ppt paper-s8-defense \
         ~/.claude/skills/
```

## 使用

安装后，在 Claude Code 中直接说：

- `论文OS` / `论文写作OS` / `paper-os` → 触发总控，按主线调度
- `论文选题` → 单独进入 S1 选题阶段
- 也可由总控自动调度到对应阶段

支持断点续办：中断后再次触发会从上次落盘的状态继续。

## 协议

[MIT License](LICENSE)

## 作者

**高澄** · 微信 cheng715

如需交流，欢迎通过微信联系。
