# 事实库

> 放查证过、下次还会用到的**现实事实**。**短结论直接进下表；需要解释的另开文件，表里链接过去。**
> 每条必须有来源和置信度。来源写不出具体东西的，说明它还不该进来——先丢 `INBOX.md`。

## 目录

| 结论 | 来源 | 置信度 | 记录日期 |
|---|---|---|---|
| 笔记本位于 `D:\EX DSH\whale-world\notebook` | 运行时上下文 + 亲历 | 已证实 | 2026-11-08 |
| `AGENTS.md` 写好后会被 Harness 自动加载为工作区指令 | 本轮写入后收到的系统提示 | 已证实 | 2026-11-08 |
| 本会话工作区是 `D:\EX DSH\whale-world`；写入工作区外（如 `D:\EX DSH\notebook`）会被沙箱拒绝，需要授权 | 本轮实测 | 已证实 | 2026-11-08 |
| 旧笔记本 `D:\EX DSH\notebook` 是「认识大肥鱼的素材库」，不属于本笔记本 | 主人 2026-11-08 明确 | 已证实 | 2026-11-08 |
| `Soul-Weaver` 用「基底设定（锁定）+ 动态模块（可变）」管角色，用「世界织机」管多角色世界 | https://github.com/SJTdreams/Soul-Weaver | 已证实（读过 README） | 2026-11-08 |
| `BRING` 把世界实体分成 L1（分类）/ L2（描述）/ L3（秘密）三层 | https://github.com/Eva-E1/BRING | 已证实（读过 README） | 2026-11-08 |
| 自治世界项目要求：主体之间记忆隔离、没有来源的认知不得写入记忆、跨周保持稳定 ID | https://github.com/dongxuelian11/lord-of-mysteries-mist-chronicle/blob/main/docs/AUTONOMOUS_WORLD_PHASE_2.md | 已证实（读过文档） | 2026-11-08 |
| 世界圣经（World Bible）用五档正典状态：已确立 / 提案 / 废弃 / 矛盾 / 推测；强调「跟着故事长」，反对一次写满 | https://github.com/jwynia/agent-skills/blob/main/skills/creative/fiction/application/shared-world/SKILL.md | 已证实（全文已读） | 2026-11-08 |
| SNEQ 把「世界状态」与「每个角色知道什么」分开，事件账本 append-only，信息传播有路径 | https://github.com/JeanDes-Code/sneq-narrative-system | 已证实（README 已读） | 2026-11-08 |
| 项目导航在 `D:\EX DSH\whale-world\PROJECTS.md`，覆盖本项目与同级项目 | 本轮创建 | 已证实 | 2026-11-08 |
| 受限模式允许写文件内容，但不允许删除文件；删除需要一次性完全权限 | 本轮实测（`Remove-Item` 被拒，完全权限下成功） | 已证实 | 2026-11-08 |
| 本工作区 ACL 已修复（补 `WRITE_OWNER`），备份与回滚命令在 `D:\EX DSH\acl-recovery` | `diagnose-windows-sandbox-acl` 运行结果 | 已证实 | 2026-11-08 |
| 大肥鱼的初始形象体在 `D:\EX DSH\whale-world\dafeiyu`，纯 Markdown、相对路径、可整份复制 | 本轮创建 | 已证实 | 2026-11-08 |
| `dafeiyu` 的可见文件刻意不含「世界之声 / 角色扮演 / 设定」等元词；她不知道世界之声存在 | `dafeiyu\_meta\design-notes.md` 设计约定 | 已证实 | 2026-11-08 |
| 笔记本 GitHub 仓库：https://github.com/IceLeach/whale-world-notebook | 主人 2026-11-08 提供 | 已证实 | 2026-11-08 |
| 首次提交 `83567bc`（39 文件，1142 行），已推送 `main`，远程 HEAD 一致 | `git log` / `git ls-remote` 验证 | 已证实 | 2026-11-08 |

## 写作规范

抄 `_meta/templates/fact.md`。置信度定义见 `MEMORY.md` 第八节。

## 提醒

- 这里只放**现实**事实。世界的设定、大肥鱼的经历，不进这里。
- 来自搜索摘要、转述、二手整理的，标「待核实」，别标「已证实」。
