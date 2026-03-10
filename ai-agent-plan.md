# 🚀 前端转 AI Agent 开发 — 完整学习计划

> **适用人群**：有前端基础（HTML/CSS/JavaScript/TypeScript/React 等），希望转型或拓展到 AI Agent 开发方向。
> 
> **开始日期**：2026-03-10
> 
> **预计完成**：2026-09-07（共 24 周）

---

## 📋 目录

- [全局概览：AI Agent 是什么？](#一全局概览ai-agent-是什么)
- [知识体系地图](#二知识体系地图)
- [第一阶段：补足 AI 基础（Week 1–4）](#三第一阶段补足-ai-基础week-14)
- [第二阶段：AI Agent 核心技能（Week 5–13）](#四第二阶段ai-agent-核心技能week-513)
- [第三阶段：全栈 AI 应用开发（Week 14–16）](#五第三阶段全栈-ai-应用开发week-1416)
- [第四阶段：进阶与生产化（Week 17–24）](#六第四阶段进阶与生产化week-1724)
- [精选学习资源](#七精选学习资源)
- [实战项目建议](#八实战项目建议)
- [前端同学的独特优势](#九前端同学的独特优势-)
- [进度打卡](#十进度打卡)

---

## 一、全局概览：AI Agent 是什么？

### 核心概念

| 概念 | 说明 |
|------|------|
| **LLM（大语言模型）** | GPT-4、Claude、Gemini 等，Agent 的"大脑" |
| **AI Agent** | 能自主规划、调用工具、完成多步任务的 AI 系统 |
| **Tool Use / Function Calling** | 让 LLM 调用外部 API、数据库、代码等 |
| **RAG（检索增强生成）** | 给 LLM 注入外部知识库，减少幻觉 |
| **Multi-Agent** | 多个 Agent 协作完成复杂任务 |
| **MCP（Model Context Protocol）** | Anthropic 推出的标准化 Agent 工具协议 |

### Agent 工作流原理

```
用户输入
   ↓
LLM 规划（思考下一步）
   ↓
工具调用（搜索 / 代码执行 / API 请求 ...）
   ↓
获取结果 → 继续规划 or 返回答案
```

---

## 二、知识体系地图

```
前端基础（已有）
    ↓
补足 Python 基础（可选，推荐）
    ↓
LLM API 使用（OpenAI / Claude / 通义等）
    ↓
Prompt Engineering（提示词工程）
    ↓
AI Agent 框架（LangChain / LangGraph / AutoGen 等）
    ↓
RAG 与向量数据库
    ↓
Agent 工具集成（MCP / Function Calling）
    ↓
全栈 AI 应用开发（前端 + Agent 后端）
    ↓
生产级部署与监控
```

---

## 三、第一阶段：补足 AI 基础（Week 1–4）

> 🎯 **目标**：理解 LLM 工作原理，学会调用 AI API，掌握 Prompt Engineering。

### Week 1–2：Python 基础 / TypeScript 异步巩固

> 💡 **前端优势**：可优先走 **Node.js/TypeScript** 路线，使用 Vercel AI SDK、LangChain.js，完全不需要 Python。

#### Python 路线（可选）

- [ ] Python 语法基础（变量、函数、类、模块）
- [ ] 异步编程（async/await）
- [ ] HTTP 请求（requests / httpx）
- [ ] 虚拟环境管理（venv / uv）

**学习资源**：
- [廖雪峰 Python 教程](https://www.liaoxuefeng.com/wiki/1016959663602400)
- [Python 官方文档（中文）](https://docs.python.org/zh-cn/3/tutorial/)

#### TypeScript 路线（推荐前端同学）

- [ ] TypeScript 高级类型（泛型、条件类型）
- [ ] Node.js 异步模型深入（Promise、async/await、Stream）
- [ ] fetch / axios 封装与错误处理

### Week 3–4：LLM API 调用 + Prompt Engineering

#### LLM API 调用

- [ ] 注册 OpenAI / Anthropic / 通义千问 账号，获取 API Key
- [ ] 理解 Chat Completions API 参数（temperature、max_tokens、stop 等）
- [ ] 实现流式输出（Streaming）
- [ ] 多轮对话上下文管理

```typescript
// 示例：TypeScript 调用 OpenAI API
import OpenAI from "openai";

const client = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });

const response = await client.chat.completions.create({
  model: "gpt-4o",
  messages: [
    { role: "system", content: "你是一个专业的编程助手" },
    { role: "user", content: "用 TypeScript 写一个冒泡排序" },
  ],
  stream: true,
});

for await (const chunk of response) {
  process.stdout.write(chunk.choices[0]?.delta?.content || "");
}
```

#### Prompt Engineering

- [ ] System Prompt 设计原则
- [ ] Few-shot Prompting（少样本提示）
- [ ] Chain of Thought（思维链）
- [ ] ReAct 模式（Reasoning + Acting）
- [ ] 结构化输出（JSON Mode）

**学习资源**：
- [Prompt Engineering Guide（中文）](https://www.promptingguide.ai/zh) ⭐
- [吴恩达 × OpenAI 提示词课程（免费）](https://learn.deeplearning.ai/courses/chatgpt-prompt-eng)

---

## 四、第二阶段：AI Agent 核心技能（Week 5–13）

> 🎯 **目标**：掌握 Agent 构建原理，能独立开发有工具调用能力的 Agent。

### Week 5–6：Function Calling / Tool Use

- [ ] 理解 Function Calling 工作原理
- [ ] 定义工具 Schema（JSON Schema）
- [ ] 处理 LLM 返回的工具调用请求
- [ ] 实现工具执行 → 结果回传的完整循环
- [ ] 并行工具调用（Parallel Tool Use）

```typescript
// 示例：Function Calling
const tools = [
  {
    type: "function",
    function: {
      name: "get_weather",
      description: "获取指定城市的实时天气",
      parameters: {
        type: "object",
        properties: {
          city: { type: "string", description: "城市名称，如：北京" },
          unit: { type: "string", enum: ["celsius", "fahrenheit"] },
        },
        required: ["city"],
      },
    },
  },
];
```

**学习资源**：
- [OpenAI Function Calling 文档](https://platform.openai.com/docs/guides/function-calling)
- [Anthropic Tool Use 文档](https://docs.anthropic.com/en/docs/tool-use)

### Week 7–9：LangChain / LangGraph

#### LangChain 核心

- [ ] LCEL（LangChain Expression Language）
- [ ] Chain 链式调用
- [ ] 内置工具集成（Tavily 搜索、计算器、Shell 等）
- [ ] Memory 记忆管理
- [ ] Agent 与 AgentExecutor

#### LangGraph（重点）

- [ ] Graph 基本概念（Node、Edge、State）
- [ ] 条件边（Conditional Edges）
- [ ] 有状态 Agent 构建
- [ ] 人机协作（Human-in-the-loop）
- [ ] 子图（Subgraph）

**学习资源**：
- [LangChain.js 官方文档](https://js.langchain.com)
- [LangGraph 官方教程](https://langchain-ai.github.io/langgraph/)
- [LangChain Academy（免费）](https://academy.langchain.com/)

### Week 10–11：RAG（检索增强生成）

- [ ] 理解 RAG 完整流程
- [ ] 文档加载与解析（PDF、Markdown、网页等）
- [ ] 文本分块策略（固定大小、递归、语义分块）
- [ ] Embedding 模型选型（OpenAI、BGE、text2vec）
- [ ] 向量数据库使用（Chroma / Qdrant / pgvector）
- [ ] 混合检索（向量 + 关键词）
- [ ] Reranking 重排序

```
文档 → 分块 → Embedding → 向量存储
                                ↓
用户提问 → Embedding → 向量检索 → Top-K 相关块 → LLM 生成答案
```

**学习资源**：
- [RAG from Scratch（吴恩达，免费）](https://learn.deeplearning.ai/courses/building-and-evaluating-advanced-rag)
- [Chroma 官方文档](https://docs.trychroma.com)
- [Qdrant 官方文档](https://qdrant.tech/documentation/)

### Week 12–13：MCP（Model Context Protocol）

- [ ] 理解 MCP 协议架构（Host / Client / Server）
- [ ] 使用现有 MCP Server（文件系统、GitHub、数据库等）
- [ ] 用 TypeScript SDK 开发自定义 MCP Server
- [ ] 在 Claude Desktop / Cursor 中集成 MCP

```typescript
// 示例：自定义 MCP Server（TypeScript）
import { Server } from "@modelcontextprotocol/sdk/server/index.js";
import { StdioServerTransport } from "@modelcontextprotocol/sdk/server/stdio.js";

const server = new Server({ name: "my-mcp-server", version: "1.0.0" }, {
  capabilities: { tools: {} },
});

// 注册工具...
```

**学习资源**：
- [MCP 官方文档](https://modelcontextprotocol.io)
- [MCP TypeScript SDK](https://github.com/modelcontextprotocol/typescript-sdk)
- [Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers)

---

## 五、第三阶段：全栈 AI 应用开发（Week 14–16）

> 🎯 **目标**：把前端技能与 Agent 后端结合，开发完整的 AI 产品。

### Week 14–15：Vercel AI SDK + Next.js

- [ ] Vercel AI SDK 核心 API（streamText、generateText、streamObject）
- [ ] Next.js App Router + Route Handler 实现 AI 接口
- [ ] 前端 useChat / useCompletion Hooks
- [ ] 流式输出 UI 实现
- [ ] 工具调用状态展示（loading、result）
- [ ] 多模态输入（图片、文件上传）

```typescript
// Next.js API Route 示例
import { streamText } from "ai";
import { openai } from "@ai-sdk/openai";

export async function POST(req: Request) {
  const { messages } = await req.json();

  const result = await streamText({
    model: openai("gpt-4o"),
    messages,
    tools: {
      searchWeb: {
        description: "搜索互联网获取最新信息",
        parameters: z.object({ query: z.string() }),
        execute: async ({ query }) => await tavilySearch(query),
      },
    },
  });

  return result.toDataStreamResponse();
}
```

**学习资源**：
- [Vercel AI SDK 文档](https://sdk.vercel.ai/docs) ⭐
- [Next.js 官方文档](https://nextjs.org/docs)

### Week 16：AI 产品 UI 设计模式

- [ ] Markdown 渲染（react-markdown + remark-gfm）
- [ ] 代码高亮（rehype-highlight / shiki）
- [ ] 思考过程展示（Thinking / Reasoning UI）
- [ ] 工具调用可视化
- [ ] 对话历史持久化（localStorage / 数据库）
- [ ] 错误处理与重试机制

**常用技术栈对比**：

| 方向 | 推荐技术栈 |
|------|-----------|
| TypeScript 全栈 | Next.js + Vercel AI SDK + LangChain.js |
| Python 后端 + 前端分离 | FastAPI + LangGraph + React |
| 快速原型验证 | Streamlit / Gradio（Python UI） |

---

## 六、第四阶段：进阶与生产化（Week 17–24）

> 🎯 **目标**：掌握多 Agent 系统、评估、监控，具备生产级 Agent 开发能力。

### Week 17–18：Multi-Agent 系统

- [ ] Multi-Agent 设计模式（Supervisor、Swarm、Pipeline）
- [ ] LangGraph 多节点 Agent 协作
- [ ] AutoGen 框架（微软）
- [ ] CrewAI（角色扮演式多 Agent）
- [ ] Agent 间通信与状态共享

**学习资源**：
- [AutoGen 官方文档](https://microsoft.github.io/autogen/)
- [CrewAI 官方文档](https://docs.crewai.com)
- [吴恩达 Multi-Agent 课程](https://learn.deeplearning.ai/courses/ai-agents-in-langgraph)

### Week 19–20：评估、监控与调试

- [ ] LangSmith 接入与 Trace 追踪
- [ ] Prompt 版本管理
- [ ] Agent 评估指标设计（准确性、延迟、Token 消耗）
- [ ] A/B 测试不同 Prompt
- [ ] 幻觉检测与事实核查

**学习资源**：
- [LangSmith 文档](https://docs.smith.langchain.com)
- [Arize Phoenix（开源 LLM 可观测性）](https://phoenix.arize.com)

### Week 21–22：安全性与边界控制

- [ ] Prompt Injection 攻击与防护
- [ ] 敏感信息过滤（PII Detection）
- [ ] 工具调用权限分级控制
- [ ] 输出内容安全审核（Guardrails）
- [ ] 速率限制与成本控制

### Week 23–24：生产部署与完整项目

#### 部署方案

| 平台 | 适用场景 |
|------|---------|
| Vercel | Next.js 全栈，最简单 |
| Railway / Render | Node.js 或 Python 后端 |
| Modal | Python 无服务器，GPU 支持 |
| Docker + 云服务器 | 完全自控，高定制 |

#### 最终项目清单

- [ ] 完成一个完整 AI Agent 项目（见下方项目建议）
- [ ] 编写完整文档（README、架构图、API 文档）
- [ ] 部署上线，获取可访问链接
- [ ] 整理 GitHub 作品集

---

## 七、精选学习资源

### 免费课程

| 资源 | 内容 | 语言 |
|------|------|------|
| [DeepLearning.AI 短课程](https://learn.deeplearning.ai/) | Agent、RAG、LangChain、MCP 等 20+ 课程 | 英文 |
| [Prompt Engineering Guide](https://www.promptingguide.ai/zh) | 最全提示词工程指南 | 中文 |
| [LangChain Academy](https://academy.langchain.com/) | LangGraph 官方系列课程 | 英文 |
| [Microsoft AI Tour](https://learn.microsoft.com/zh-cn/ai/) | Azure AI、AutoGen 等 | 中文 |

### 推荐书籍

| 书名 | 作者 | 推荐理由 |
|------|------|---------|
| *AI Engineering* | Chip Huyen | 2025 新书，生产级 LLM 应用必读 |
| *Build LLM-Powered Applications* | Valentina Alto | 实战向，覆盖 RAG、Agent |
| *Hands-On Large Language Models* | Jay Alammar | 图文并茂，原理讲解清晰 |

### 必备工具

| 工具 | 用途 | 链接 |
|------|------|------|
| Cursor / GitHub Copilot | AI 辅助编程 | [cursor.sh](https://cursor.sh) |
| LangSmith | Agent 调试与追踪 | [smith.langchain.com](https://smith.langchain.com) |
| OpenAI Playground | 快速测试 Prompt | [platform.openai.com/playground](https://platform.openai.com/playground) |
| Anthropic Console | Claude API 测试 | [console.anthropic.com](https://console.anthropic.com) |
| Tavily | Agent 联网搜索 API | [tavily.com](https://tavily.com) |

### 优质社区

- [LangChain Discord](https://discord.gg/langchain)
- [Hugging Face 论坛](https://discuss.huggingface.co/)
- GitHub Topics: `llm-agent` | `langchain` | `mcp` | `rag`

---

## 八、实战项目建议

> 建议每完成一个项目都推送到 GitHub，逐步积累作品集。

| 难度 | 项目 | 核心技术 |
|------|------|---------|
| 🟢 Level 1 | 带记忆的多轮对话聊天机器人 | LLM API + 对话历史管理 |
| 🟢 Level 2 | 能联网搜索的 AI 助手 | Function Calling + Tavily API |
| 🔵 Level 3 | 本地知识库问答系统（RAG） | Embedding + 向量数据库 + LangChain |
| 🔵 Level 4 | 代码解读/生成 Agent | GitHub API + MCP + LangGraph |
| 🟠 Level 5 | 自动化研究报告 Agent | Multi-step Agent + RAG + 文档生成 |
| 🔴 Level 6 | 多 Agent 协作系统 | LangGraph / CrewAI + Multi-Agent 设计 |

---

## 九、前端同学的独特优势 💪

| 前端技能 | AI Agent 中的应用 |
|---------|----------------|
| TypeScript | 直接用 LangChain.js、Vercel AI SDK，无需 Python |
| React / Next.js | 快速构建 AI 应用全栈原型 |
| 异步编程 | Agent 流式输出、并发工具调用非常自然 |
| API 调用经验 | Function Calling 实现直觉化 |
| UI/UX 能力 | AI 产品用户体验是差异化核心竞争力 |
| 状态管理 | 复杂 Agent 对话状态管理得心应手 |

---

## 十、进度打卡

### 第一阶段（Week 1–4）

- [ ] Week 1：Python 基础 / TypeScript 异步
- [ ] Week 2：Python 进阶 / Node.js 异步深入
- [ ] Week 3：LLM API 调用 + 流式输出
- [ ] Week 4：Prompt Engineering 全面掌握

### 第二阶段（Week 5–13）

- [ ] Week 5：Function Calling 基础
- [ ] Week 6：Function Calling 进阶 + 并行调用
- [ ] Week 7：LangChain 核心概念
- [ ] Week 8：LangGraph 基础
- [ ] Week 9：LangGraph 进阶 + Human-in-the-loop
- [ ] Week 10：RAG 基础 + 向量数据库
- [ ] Week 11：RAG 进阶 + 混合检索
- [ ] Week 12：MCP 协议理解 + 使用现有 Server
- [ ] Week 13：开发自定义 MCP Server

### 第三阶段（Week 14–16）

- [ ] Week 14：Vercel AI SDK + Next.js 接入
- [ ] Week 15：完整 AI 应用前后端联调
- [ ] Week 16：AI 产品 UI 优化 + 对话持久化

### 第四阶段（Week 17–24）

- [ ] Week 17：Multi-Agent 设计模式
- [ ] Week 18：LangGraph / CrewAI 多 Agent 实战
- [ ] Week 19：LangSmith 监控接入
- [ ] Week 20：Agent 评估体系建立
- [ ] Week 21：安全性与 Prompt Injection 防护
- [ ] Week 22：生产部署实践
- [ ] Week 23：完整项目开发
- [ ] Week 24：项目上线 + 作品集整理 🎉

---

## 附：推荐技术栈总览

```
┌─────────────────────────────────────────────┐
│              AI Agent 全栈技术栈              │
├─────────────┬───────────────────────────────┤
│  前端 UI    │ Next.js + Tailwind + shadcn/ui │
├─────────────┼───────────────────────────────┤
│  AI SDK     │ Vercel AI SDK / LangChain.js   │
├─────────────┼───────────────────────────────┤
│  Agent 框架 │ LangGraph / CrewAI / AutoGen   │
├─────────────┼───────────────────────────────┤
│  LLM        │ GPT-4o / Claude 3.5 / 通义     │
├─────────────┼───────────────────────────────┤
│  工具协议   │ MCP / Function Calling         │
├─────────────┼───────────────────────────────┤
│  向量数据库 │ Qdrant / Chroma / pgvector     │
├─────────────┼───────────────────────────────┤
│  监控       │ LangSmith / Arize Phoenix      │
├─────────────┼───────────────────────────────┤
│  部署       │ Vercel / Railway / Docker      │
└─────────────┴───────────────────────────────┘
```

---

*最后更新：2026-03-10 | 作者：mf-Peace*