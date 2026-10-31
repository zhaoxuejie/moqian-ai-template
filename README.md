# moqian-ai-template
陌阡 AI 工程治理模板

> 面向AI Agent、LLM应用开发的自研工程框架，文档先行、增量任务卡、版本快照归档、会话提示词本地托管。

## 核心理念
全部流程、规范、文档模板、启动提示词、阶段指令都纳入Git管理，不依赖云端对话记忆。
严格顺序：骨架初始化 → PRD → 架构ADR → 接口契约 → 增量分批任务卡 → 逐任务开发验收 → 版本发布/MAJOR归档。

## 目录概览
```
moqian-ai-template/
├── docs/                # 核心文档模板、提示词库、任务卡、归档快照
├── scripts/             # 版本发布脚本
├── tests/               # 单元测试、评估集存放目录
└── .github/workflows/   # CI：markdown、shell静态检查
```

### docs 说明
- `prompt.md`：全局基座提示词，新开AI会话首段粘贴
- `stage-prompts.md`：分阶段指令库（初始化/PRD/架构/接口/拆任务/开发/发布）
- `prd.md` / `architecture.md` / `api-contract.md`：业务标准文档模板
- `tasks/`：单文件任务卡体系，索引看板 + 独立任务文件
- `archive/`：MAJOR破坏性大版本快照归档目录

## 使用流程
1. 复制本模板作为新项目起点
2. 新开会话：粘贴 `docs/prompt.md`
3. 从 `stage-prompts.md` 选取当前阶段指令追加
4. 按框架顺序推进，禁止跳流程、禁止一次性全量生成任务

## 规范
- Trunk-Based 主干开发
- ADR追加式记录，不修改历史决策
- 任务卡：一个任务一个独立文件，增量拆分
- MAJOR版本先归档快照，再迭代新版本文档
- 废弃接口标记 deprecated + 迁移指引

## CI
PR / push 自动检查 Markdown 语法、Shell脚本语法
