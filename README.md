# moqian-ai-template
陌阡 AI 工程治理模板

> 面向 AI Agent、LLM 应用开发的工程治理模板：把"先文档后代码、增量任务卡、决策留痕、上下文本地托管"固化为一套开箱即用的目录结构、文档模板与分阶段提示词。帮助你在 AI 协作开发中**锁定需求、抑制需求漂移、避免 AI 自由发挥跑偏**；不熟悉 AI 编程协作方式的人，也能照着固定阶段把项目从 0 规范推进到 1。

## 它解决什么问题

直接让 AI 从零写代码，常见问题是需求越做越偏、上下文随会话丢失、任务一次性炸开、决策和版本无从追溯。本模板用固定流程和文档闸门逐一约束：

- **防需求漂移**：需求、架构、接口先落成文档并经人工确认，才允许写业务代码；AI 不得跳过阶段自由发挥。
- **上下文不丢失**：全部规范、提示词、决策记录随仓库纳入 Git 本地托管，不依赖云端对话记忆；新会话粘贴 `docs/prompt.md` 即可恢复完整框架上下文。
- **新手友好**：`docs/stage-prompts.md` 提供每个阶段可直接复制的指令，不用自己总结 AI 协作方法论，按阶段发送即可。
- **抑制范围蔓延**：任务一卡一文件、增量分批拆解，前置依赖全部 `done` 才允许启动下一个任务。
- **决策与版本可追溯**：ADR 只追加不修改；MAJOR 破坏性升级先归档快照；`CHANGELOG.md` + 发布脚本管理版本与标签。

## 核心理念

1. 全部流程、规范、文档模板、启动提示词、阶段指令都纳入 Git 管理，不依赖云端对话记忆。
2. 严格顺序推进：骨架初始化 → PRD → 架构 ADR → 接口契约 → 增量分批任务卡 → 逐任务开发验收 → 版本发布 / MAJOR 归档。
3. 每个阶段都设人工审阅闸门，上一阶段未确认不进入下一阶段；禁止跳流程、禁止一次性全量生成任务、禁止无文档直接写代码。

## 适合谁

- 用 AI Agent / LLM 从零启动新项目的开发者与独立创业者
- 担心 AI 开发过程中需求跑偏，希望先把需求和边界固化成文档
- 不熟悉 AI 协作流程，需要现成规范、目录和阶段指令库
- 需要保留决策记录、任务验收留痕与版本快照治理的小团队或个人项目

## 目录结构

```
moqian-ai-template/
├── README.md                          # 项目说明（本文件）
├── AGENTS.md                          # 面向 AI Agent 的开发运行约束（任务拆分、文档规范、任务卡规则）
├── CONTRIBUTING.md                    # 分支、提交信息、PR 评审与文件命名规范
├── CHANGELOG.md                       # 版本变更日志（Keep a Changelog + 语义化版本）
├── LICENSE                            # MIT 许可证
├── .env.example                       # 环境变量样例，复制为 .env 填写，禁止提交真实密钥
├── .gitignore                         # 忽略密钥、依赖、构建产物、日志与编辑器文件
├── .gitattributes                     # 统一文本文件 LF 换行，保证跨平台与 Linux CI 一致
├── .markdownlint.json                 # Markdown 静态检查规则配置
├── .github/
│   └── workflows/ci.yml               # CI：markdownlint 文档检查 + shellcheck 脚本检查
├── docs/
│   ├── readme.md                      # docs 目录索引与语义化版本规则说明
│   ├── prompt.md                      # 全局基座提示词：新开会话首段粘贴，恢复全套框架规则
│   ├── stage-prompts.md               # 分阶段指令库：初始化 / PRD / 架构 / 接口 / 拆任务 / 开发 / 升级 / 发布 / 评审
│   ├── prd.md                         # 产品需求文档模板：目标、范围 In/Out、场景、功能、里程碑
│   ├── architecture.md                # 架构方案 + ADR 决策记录模板（历史 ADR 只追加）
│   ├── api-contract.md                # 接口契约模板：版本标记、deprecated 与迁移指引
│   ├── tasks/
│   │   ├── readme.md                  # 任务索引看板（仅登记索引，不存放任务正文）
│   │   └── task-00x-xxx.md            # 单任务卡模板：执行步骤、验收标准、评测方案
│   └── archive/
│       └── .gitkeep                   # MAJOR 破坏性版本冻结快照归档目录，归档后只读
├── scripts/
│   └── release.sh                     # 语义化版本发布脚本：更新 CHANGELOG 并给出打 tag 指引
└── tests/
    └── .gitkeep                       # 单元测试、契约测试、LLM 评估集存放目录
```

## 标准工作流程

每个阶段产出对应文档，**必须经人工审阅确认后**才进入下一阶段；阶段指令见 `docs/stage-prompts.md`。

| 阶段 | 产出物 | 进入下一阶段的前提 |
| ---- | ---- | ---- |
| 0 初始化骨架 | 目录结构与初始文件 | 骨架就绪，等待项目需求 |
| 1 编写 PRD | `docs/prd.md` | PRD 人工审阅通过 |
| 2 架构 + ADR | `docs/architecture.md` | 架构方案人工审阅通过 |
| 3 接口契约 | `docs/api-contract.md` | 接口文档人工审阅通过 |
| 4 增量拆任务卡 | 第一批 3～5 张任务卡 + 更新索引 | 任务卡人工审阅确认入库 |
| 5 逐任务开发验收 | 代码 + 配套测试，任务卡标记 `done` | 验收标准与评测方案全部通过 |
| 7 版本发布 | 更新 `CHANGELOG.md`，执行发布脚本并打 tag | 当前里程碑任务全部 `done` |
| 6 MAJOR 升级 | `docs/archive/` 快照 + 更新文档 + 新增 ADR | 发生不兼容破坏性变更时触发 |

### 两种启动方式

**手动方式：** 复制本模板作为新项目起点，新开会话先粘贴 `docs/prompt.md`，再按 `docs/stage-prompts.md` 选择当前阶段指令。

**一键方式（推荐）：** 使用配套 Agent Skill [moqian-project-scaffold](https://github.com/zhaoxuejie/moqian-project-scaffold)，只需提供项目名，即可自动克隆模板、清理示例、重置占位文档并初始化独立 Git 历史。

## 工程规范要点

- **先文档、后代码**：PRD、架构、接口未确认前，禁止编写业务代码。
- **任务卡规则**：一个任务一个独立文件；增量拆分，禁止一次性全量拆解；前置依赖全部 `done` 才可启动；验收评测不通过不能标记 `done`。
- **ADR 只追加**：历史决策不修改、不删除；新增决策独立成条。
- **MAJOR 归档**：破坏性升级先把当前稳定版全套文档快照存入 `docs/archive/vX.Y/`，再更新活跃文档。
- **废弃标记**：废弃接口或功能必须标记 `deprecated` 并附迁移指引，不得直接删除。
- **分支与提交**：Trunk-Based 主干开发，改动走分支 + PR；提交信息格式 `类型(范围): 简短描述`。细则见 [CONTRIBUTING.md](CONTRIBUTING.md)。
- **文件命名**：仓库顶层文件首字母大写；`docs` 内文档与任务卡统一小写 kebab-case。

## CI 质量检查

每次 push 到 `main` 或提交 PR，GitHub Actions 自动执行：

- `lint-md`：markdownlint 检查全部 Markdown 文档规范。
- `lint-shell`：shellcheck 检查 `scripts/` 下脚本语法。

## 配套技能

- [moqian-project-scaffold](https://github.com/zhaoxuejie/moqian-project-scaffold)：基于本模板一键初始化新项目骨架的 Agent Skill，自动完成克隆、清理示例、重置占位与独立 Git 初始化。

## License

MIT，Copyright (c) 2026 moqian。
