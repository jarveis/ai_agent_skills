---
name: spec-kit-sdd
description: 当需要用 Spec Kit 的“规格驱动开发”流程，让 Claude Code 先写清需求、计划和任务，再执行实现时使用。
---

# spec-kit-sdd

## 什么时候使用

在以下任务中使用这个 skill：

- 用户要做一个新功能，但需求还比较粗。
- 需要避免 Agent 直接进入“vibe coding”。
- 需要先沉淀 spec、plan、tasks，再写代码。
- 团队希望功能实现过程可审查、可追踪、可复用。
- 任务涉及多步骤、多文件、多角色协作，直接开写容易跑偏。

## 来源

- 仓库：<https://github.com/github/spec-kit>
- 许可证：MIT License
- 原始形式：开源 Spec-Driven Development 工具包 / Specify CLI / Agent 集成
- 本次整理名称：spec-kit-sdd

## 核心原则

1. **先规格，后实现**  
   不要一上来写代码。先明确用户场景、目标、边界和验收条件。

2. **先写 what 和 why，再谈 how**  
   `/speckit.specify` 阶段聚焦需求本身，不急着决定技术栈。

3. **计划必须连接到规格**  
   `/speckit.plan` 不是随便写架构，而是把规格翻译成可实现的技术路线。

4. **任务必须可执行**  
   `/speckit.tasks` 应输出可以逐项完成、验证和追踪的任务。

5. **实现必须回到规格验证**  
   `/speckit.implement` 之后要检查实现是否覆盖 spec、plan 和 tasks。

## Claude Code 安装 / 接入方式

Spec Kit 提供 Specify CLI。官方 README 中的安装方式需要 `uv`，并从 GitHub 安装指定版本：

```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@vX.Y.Z
```

把 `vX.Y.Z` 替换为 GitHub Releases 中的最新版本。

初始化 Claude Code 项目：

```bash
specify init my-project --integration claude
cd my-project
```

如果是在已有项目里初始化：

```bash
specify init . --integration claude
```

初始化后，Claude Code 中通常会出现这些结构化命令：

```text
/speckit.constitution
/speckit.specify
/speckit.plan
/speckit.tasks
/speckit.implement
/speckit.analyze
/speckit.clarify
/speckit.checklist
/speckit.converge
```

Spec Kit 的 Claude Code 集成会使用 `.claude/skills`。如果只是安装本整理版 skill，可放到：

```text
.claude/skills/spec-kit-sdd/SKILL.md
```

用户级安装可放到：

```text
~/.claude/skills/spec-kit-sdd/SKILL.md
```

> 说明：本 `SKILL.md` 是对 Spec Kit 使用流程的中文整理。完整命令、模板和脚手架仍建议通过官方 Specify CLI 安装。

## 工作流程

### 1. 建立项目原则

适合新项目或重要模块：

```text
/speckit.constitution Create principles focused on code quality, testing standards, user experience consistency, and performance requirements
```

输出项目长期遵守的原则，例如测试标准、性能要求、用户体验边界和代码质量约束。

### 2. 创建规格

让 Agent 描述要构建的功能，重点写 what 和 why：

```text
/speckit.specify 构建一个图片相册管理功能，用户可以按日期分组照片，支持拖拽调整相册顺序，照片仅保存在本地。
```

这一阶段不要急着指定框架、数据库或具体实现。

### 3. 创建技术计划

在规格稳定后再提供技术选型：

```text
/speckit.plan 使用 Vite，尽量少引入库；元数据保存在本地 SQLite；图片不上传到远程服务。
```

计划应该说明架构、数据模型、关键约束和风险。

### 4. 拆解任务

```text
/speckit.tasks
```

生成可以逐项执行的任务清单。每个任务应该有清晰的输入、输出和完成条件。

### 5. 执行实现

```text
/speckit.implement
```

执行任务时，要求 Agent 按 tasks 顺序推进，并在关键节点运行测试或检查。

### 6. 做一致性检查

在实现前后都可以运行：

```text
/speckit.analyze
/speckit.converge
```

用来发现 spec、plan、tasks 和代码之间的不一致。

## 输出要求

使用本 skill 时，最终汇报至少包含：

```markdown
## Spec Kit 工作结果

- 使用阶段：constitution / specify / plan / tasks / implement / analyze / converge
- 生成或更新的规格文件：...
- 生成或更新的计划文件：...
- 生成或更新的任务文件：...
- 已完成任务：...
- 剩余任务：...
- 已验证内容：...
- 风险或待确认问题：...
```

## 使用示例

```text
使用 spec-kit-sdd，帮我把“用户可以收藏文章并按标签检索”这个想法整理成规格、计划和任务。先不要写代码，先跑 specify、plan、tasks 的流程。
```

```text
使用 spec-kit-sdd，检查当前 specs 目录里的规格、计划和任务是否一致，再告诉我实现前还缺哪些信息。
```

## 最终检查清单

- [ ] 是否先写清楚用户场景和验收条件？
- [ ] 是否把技术实现延后到 plan 阶段？
- [ ] tasks 是否可执行、可验证、可追踪？
- [ ] implement 是否按任务推进，而不是随意改代码？
- [ ] 是否检查 spec、plan、tasks 和代码之间的一致性？
- [ ] 是否列出未解决问题和剩余风险？
