# DeepSeek Harness Prompts

[English](README.md) | 中文

为 DeepSeek 模型的智能体运行时 [DeepSeek Harness (DSH)](https://github.com/deepseek-ai) 精心整理的系统 prompt 合集。每个文件都是完整、自包含的 prompt 包 —— 包含系统 prompt、工具定义和运行时上下文 —— 可直接用作编程智能体会话的 harness 指令。

## 模式一览

| 文件 | 模式 | 大小 | 说明 |
|------|------|------|------|
| [`deepseek-harness-standard.md`](deepseek-harness-standard.md) | **标准模式（Standard）** | ~35 KB | 完整的生产级 harness prompt。详尽的行为规范，外加所有工具的完整 JSON Schema 定义（`read`、`write`、`edit`、`glob`、`grep`、`bash`、后台任务、子智能体、目标、工作流、网络搜索等）。适合需要最强能力和精确工具调用控制的场景。 |
| [`deepseek-harness-simple.md`](deepseek-harness-simple.md) | **极简模式（Simple）** | ~4 KB | 最小化 harness prompt。紧凑的系统 prompt，采用 XML 风格 `<invoke>` 工具调用格式，仅含两个工具（`bash`、`str_replace_editor`）。适合轻量级部署、基准测试或小上下文窗口。 |
| [`deepseek-harness-creative.md`](deepseek-harness-creative.md) | **创造模式（Creative）** | ~42 KB | 可自我扩展的 harness prompt。在标准工具集之上，智能体可以读取并修改自己运行的 harness：以 Cordis 插件行的方式组合能力、编写智能体预设（agent presets）、定义并运行带 Host/Client 双端的动态 Cordis 插件。适合实验智能体自定义工具、UI 插槽和运行时扩展。 |
| [`deepseek-harness-ptc.md`](deepseek-harness-ptc.md) | **PTC 模式** | ~38 KB | 程序化工具调用（Programmatic Tool Calling）。`run_code` 是唯一可直接调用的工具；其余所有工具都在异步 TypeScript 程序内通过 `await tools.name(args)` 触达。工具以带类型的 `ToolArgsMap` 接口声明（而非 JSON Schema），安全的只读调用可在 `Promise.all` 下并发，且中间工具结果不会进入对话 —— 只有程序打印或返回的内容才会回来。适合长工具链调用和保持上下文精简。 |

## Prompt 结构

每个文件遵循相同的布局：

1. **System Prompt** —— 身份、环境事实，以及各工具的行为规则。
2. **Tool Definitions** —— 暴露给模型的工具 schema（格式因模式而异：JSON Schema、XML invoke 块，或 PTC 模式中的 TypeScript `ToolArgsMap` 接口）。
3. **Runtime Context Snapshot** —— 会话的文件沙箱策略和审批策略。
4. **System Reminder** —— 可用技能（skills）目录及技能加载规则。

## 如何选择模式

- **用标准模式**：通用智能体编程任务，模型需要完整的工具面和严格的使用规则。
- **用极简模式**：希望 prompt 占用尽可能小，或评估模型原生的工具调用能力。
- **用创造模式**：任务涉及扩展 harness 本身 —— 构建动态插件、自定义工具或会话级 UI。
- **用 PTC 模式**：任务包含大量相互依赖的工具调用：模型在一个程序中编排调度，并发执行独立的读取，最终只返回提炼后的结果。

## 许可

可在你自己的 harness 实验和部署中自由使用这些 prompt。
