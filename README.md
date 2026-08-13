# DeepSeek Harness Prompts

English | [中文](README.zh-CN.md)

A curated collection of system prompts for [DeepSeek Harness (DSH)](https://github.com/deepseek-ai), the agentic runtime for DeepSeek models. Each file is a complete, self-contained prompt bundle — system prompt, tool definitions, and runtime context — ready to be used as the harness instructions for a coding-agent session.

## Modes

| File | Mode | Size | Description |
|------|------|------|-------------|
| [`deepseek-harness-standard.md`](deepseek-harness-standard.md) | **Standard** | ~35 KB | Full production-grade harness prompt. Detailed behavioral guidelines plus complete JSON Schema definitions for all tools (`read`, `write`, `edit`, `glob`, `grep`, `bash`, background jobs, subagents, goals, workflows, web search, and more). Best for maximum capability and precise tool-use control. |
| [`deepseek-harness-simple.md`](deepseek-harness-simple.md) | **Simple** | ~4 KB | Minimal harness prompt. A compact system prompt with an XML-style `<invoke>` tool-call format and only two tools (`bash`, `str_replace_editor`). Best for lightweight setups, benchmarking, or small context windows. |
| [`deepseek-harness-creative.md`](deepseek-harness-creative.md) | **Creative** | ~42 KB | Self-extending harness prompt. On top of the standard toolset, the agent can read and modify the harness it runs on: composing capabilities as Cordis plugin rows, authoring agent presets, and defining/running dynamic Cordis plugins with Host/Client surfaces. Best for experimenting with agent-defined tools, UI slots, and runtime extensions. |
| [`deepseek-harness-ptc.md`](deepseek-harness-ptc.md) | **PTC** | ~38 KB | Programmatic Tool Calling. `run_code` is the only directly callable tool; every other tool is reached from inside an async TypeScript program via `await tools.name(args)`. Tools are declared as a typed `ToolArgsMap` interface instead of JSON Schemas, safe read-only calls can overlap under `Promise.all`, and intermediate tool results stay out of the conversation — only what the program prints or returns comes back. Best for long tool chains and keeping context small. |

## Prompt Structure

Each file follows the same layout:

1. **System Prompt** — identity, environment facts, and per-tool behavioral rules.
2. **Tool Definitions** — the tool schemas exposed to the model (format varies by mode: JSON Schema, XML invoke blocks, or a TypeScript `ToolArgsMap` interface in PTC mode).
3. **Runtime Context Snapshot** — file-sandbox policy and approval policy for the session.
4. **System Reminder** — the available-skills catalog and skill-loading rules.

## Choosing a Mode

- **Use Standard** for general-purpose agentic coding where the model needs the complete tool surface with strict usage rules.
- **Use Simple** when you want the smallest possible prompt footprint or are evaluating raw model tool-calling ability.
- **Use Creative** when the task involves extending the harness itself — building dynamic plugins, custom tools, or session-scoped UI.
- **Use PTC** when tasks involve many dependent tool calls: the model orchestrates them in one program, parallelizes independent reads, and returns only the distilled result.

## License

Use these prompts freely in your own harness experiments and deployments.
