# my-llm-wiki

> 让 AI Agent 无缝读取、维护和复用你的微服务接口知识库。

`my-llm-wiki` 是一个专为 AI 编程助手（如 Codex, Claude Code, Cursor 等）设计的知识库生成技能（Skill）与规范。它融合了 Karpathy 的 LLM Wiki 编译范式，能够将你庞大、复杂的微服务代码，编译成结构化、原子化、互链的 Markdown 知识库。

不需要向量数据库，不需要 MCP，纯文件 + 搜索即可让 AI 拥有“上帝视角”。

## ✨ 核心特性

- **🎯 入口即页面**：无论是 Controller 方法还是 Consumer Listener，一个入口对应一个独立 Markdown 文件，拒绝大而全的臃肿文档。
- **🗂️ 两级索引导航**：一级索引按服务划分，二级索引列出服务下的所有接口。AI 按图索骥，按需读取，极大节省 Token。
- **📖 完整业务规则**：强制覆盖所有校验、分支、异常与副作用。不掐头去尾，确保 AI 不会基于缺失的上下文“幻觉”。
- **📊 Mermaid 流程图**：用流程图替代冗长的代码实现说明，原生兼容 Obsidian，人类和 AI 都能一眼看懂执行路径。
- **📝 服务级日志隔离**：每个服务拥有独立的 `log.md`，记录知识库的增量变更，避免全局日志膨胀。
- **🧠 知识沉淀 (Synthesis)**：AI 在日常分析中产生的跨接口洞察，可以写回 `synthesis/` 目录，形成团队共享的“外脑”。
- **🛡️ 零依赖**：生成的都是纯 Markdown 文件，你可以直接提交到 Gitlab/Github，或者用 Obsidian 打开作为人类阅读文档。

## 💡 设计哲学 (The LLM Wiki Way)

传统 RAG 每次查询都要重新检索、推导，而 `my-llm-wiki` 遵循 **"Compile once, read every day"（编译一次，日常阅读）** 的理念。
代码是原始素材（Raw Sources），我们让 AI 在后台将其编译成结构化知识（Wiki）。日常开发时，AI 只需读取编译好的 Wiki，不仅准确率极高，还能通过 `synthesis` 不断积累团队的架构认知。

## 🤖 如何让 Codex / Cursor 使用它？

知识库生成或拉取到本地后（可以是项目内目录，也可以是独立文件夹），你只需要在项目的 `AGENTS.md`（或 `.cursorrules` 等）中配置读取指引：

```markdown
## 知识库

接口知识库在 `/your-absolute-path/my-llm-wiki/`。
在回答涉及接口、业务规则、调用链的问题时，按以下顺序按需读取：

1. `index.md` — 确定目标服务
2. `services/<service>/index.md` — 确定目标入口
3. 只读命中的具体页面，不要批量读取
4. 跨接口问题优先读 `synthesis/` 下的页面