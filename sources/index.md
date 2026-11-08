# 资料源

> 好用的链接、文档、仓库。**写清「可信到什么程度」和「能用来干什么」**，光贴链接等于没记。

## 目录

| 资料 | 链接 | 可信度 | 能用来干什么 | 备注 |
|---|---|---|---|---|
| Soul-Weaver（灵魂织者） | https://github.com/SJTdreams/Soul-Weaver | 高（README 全文已读） | 世界织机：多角色独立记忆、事件进入/离开、画像区（对角色的主观印象）、基底设定锁定 + 动态模块可变 | 参考其「锁定 vs 可变」的分层 |
| BRING v2 | https://github.com/Eva-E1/BRING | 高（README 全文已读） | 分层世界观 L1/L2/L3、图谱记忆、后台导演推进剧情、NPC 概率系统 | 只取「分层 + 导演」思路，不引入其代码 |
| 自治世界 · 第二阶段 | https://github.com/dongxuelian11/lord-of-mysteries-mist-chronicle/blob/main/docs/AUTONOMOUS_WORLD_PHASE_2.md | 高（文档已读） | 持续角色、私有记忆投影、无来源不写入、稳定 ID、世界账本 | 本笔记本「记忆隔离 / 来源」原则的重要参考 |
| GitHub · long-horizon-memory 协议 | https://github.com/github/gh-aw/blob/main/.github/workflows/shared/long-horizon-memory.md | 中（只取到链接，未全文读） | 长周期 agent 记忆的更新/压缩约定 | 待核实后再引用细节 |
| World Bible Management Skill | https://github.com/jwynia/agent-skills/blob/main/skills/creative/fiction/application/shared-world/SKILL.md | 高（全文已读） | 正典状态五档（已确立/提案/废弃/矛盾/推测）、世界条目模板、反「百科式堆砌」、世界圣经跟着故事长 | 本轮 `world/canon.md` 与条目模板的主要参考 |
| SNEQ 叙事状态引擎 | https://github.com/JeanDes-Code/sneq-narrative-system | 高（README 全文已读） | 把「世界状态」和「每个角色知道什么」分开；事件账本 append-only；发明在 promotion 前保持暂定；信息传播有路径 | 本轮 `world/knowledge.md` 的主要参考 |
| Cognition · Agent Memory Repo | https://cognition.com/agent-memory-repo | 高（上一代笔记本曾完整读过；本轮未复读） | `MEMORY.md` 入口、`[source]` 标注、双链、定期清理 | 结构的主要参考 |
| Claude Code · Memory 文档 | https://code.claude.com/docs/en/memory.md | 高（上一代笔记本曾读过） | 入口文件加载上限（前 200 行 / 25KB），`@path` 导入 | 本笔记本把入口压在 150 行内 |

## 取用记录

- **2026-11-08**：GitHub 网页版抓取只返回导航；要正文走 `raw.githubusercontent.com/<owner>/<repo>/<branch>/README.md`。
- **2026-11-08**：本次搜索方向是「AI 角色扮演 + 持久世界 + 记忆架构」，上面四个项目是筛出来的。

## 收录标准

能反复用的才收。一次性的搜索结果、看完就忘的新闻，别占位置。

可信度三档：**高**（官方文档、一手代码，自己读过）、**中**（读过一部分，或权威二手）、**低**（只有摘要或转述）。没读过的标「未评估」。
