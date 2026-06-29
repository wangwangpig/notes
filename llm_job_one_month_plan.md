# 大模型算法开发 / 应用开发 1 个月学习计划

> 目标：面向大模型算法开发、应用开发、Agent/RAG 相关实习或初级岗位，在 1 个月内建立知识框架，完成一个能写进简历的 RAG Agent Harness 项目，并补充一个小规模 LoRA / 开源模型实验。

## 1. 学习主线

建议不要把大模型学习拆成很多孤立知识点，而是按下面这条线走：

```text
LLM 基础
→ Prompt / 结构化输出
→ RAG
→ Tool Calling
→ Agent
→ Harness / Eval / Trace
→ LoRA / 开源模型部署
```

如果时间只有 1 个月，优先级是：

1. RAG 项目闭环
2. Agent / Tool Calling
3. Harness 工程化
4. LLM 原理面试基础
5. LoRA / 本地模型实验

## 2. Datawhale 项目学习顺序

这几个项目分工不同，建议不要同时啃。

| 项目 | 作用 | 学习重点 |
|---|---|---|
| [all-in-rag](https://github.com/datawhalechina/all-in-rag) | RAG 应用全栈 | 文档解析、切分、Embedding、向量库、Rerank、Graph RAG、评估 |
| [hello-agents](https://github.com/datawhalechina/hello-agents) | Agent 入门 | ReAct、Tool Calling、Function Calling、多 Agent、Agent 项目实践 |
| [happy-llm](https://github.com/datawhalechina/happy-llm) | 大模型原理 | Transformer、Attention、Tokenizer、预训练、SFT、LoRA、评估 |
| [self-llm](https://github.com/datawhalechina/self-llm) | 开源模型部署/微调 | Transformers、PEFT、LoRA/QLoRA、vLLM、Gradio、本地模型部署 |

推荐顺序：

```text
all-in-rag → hello-agents → happy-llm 穿插补原理 → self-llm 最后做加分实验
```

如果更偏算法开发岗：

```text
happy-llm → all-in-rag → self-llm → hello-agents
```

如果更偏大模型应用开发岗：

```text
all-in-rag → hello-agents → happy-llm → self-llm
```

## 3. 这几个项目的欠缺

Datawhale 项目覆盖面很好，但和更工程化的 Agent Runtime / Harness 相比，还需要你自己补几块。

| 能力 | 覆盖情况 | 需要你额外补什么 |
|---|---|---|
| LLM 原理 | happy-llm 覆盖较好 | 面试表达、公式理解、推理流程 |
| RAG | all-in-rag 覆盖较好 | 项目化、评估、Trace、失败分析 |
| 微调部署 | self-llm 覆盖较好 | 小规模 LoRA 实验和效果对比 |
| Agent | hello-agents 覆盖较好 | Runtime 化、状态管理、错误处理 |
| Harness | 不够系统 | AgentState、ToolRegistry、ToolResponse、TraceLogger、EvalRunner |
| Eval | 有但不够项目化 | 自动评测集、指标统计、版本对比 |
| 权限/安全 | 教程里一般较少 | Tool 白名单、危险操作确认、Step Limit、Timeout |
| Context Engineering | 部分涉及 | 上下文压缩、历史摘要、工具输出裁剪 |
| MCP / Skills / CLI | 部分涉及 | 了解概念即可，可作为扩展设计 |

## 4. 最终项目建议

建议最终做一个：

```text
Mini RAG Agent Harness
```

项目目标：

用学习路线 PDF、岗位 JD、Datawhale 文档或自己的笔记作为知识库，构建一个支持检索、问答、引用、工具调用、日志追踪和自动评估的 RAG Agent。

核心能力：

- 文档解析：PDF / Markdown / TXT
- 文本切分：chunk size、chunk overlap、semantic chunking
- Embedding：本地模型或 API
- 向量库：FAISS / Chroma
- 检索：Top-K、BM25、Hybrid Search
- Rerank：提高召回片段质量
- 生成：基于检索上下文生成答案
- 引用：答案附带来源 chunk
- Agent：将 search、rerank、generate 封装成工具
- Harness：统一调度工具、记录状态、处理失败
- Eval：批量跑问题集，统计指标

推荐目录：

```text
mini-rag-agent-harness/
├── README.md
├── requirements.txt
├── app.py
├── data/
│   ├── docs/
│   └── eval_questions.jsonl
├── rag/
│   ├── loader.py
│   ├── splitter.py
│   ├── embedder.py
│   ├── vector_store.py
│   └── retriever.py
├── tools/
│   ├── search_docs.py
│   ├── rerank_docs.py
│   ├── query_rewrite.py
│   └── generate_answer.py
└── harness/
    ├── state.py
    ├── runtime.py
    ├── tool_registry.py
    ├── tool_response.py
    ├── trace_logger.py
    ├── guard.py
    └── eval_runner.py
```

## 5. Harness 怎么融入

普通 RAG 流程：

```text
用户问题 → 检索 → 拼 Prompt → 调 LLM → 输出答案
```

加入 Harness 后：

```text
用户问题
→ 创建 AgentState
→ 调用 search_docs 工具
→ 检查召回结果
→ 召回差则 query_rewrite
→ 调用 rerank_docs
→ 调用 generate_answer
→ 校验引用和格式
→ 记录 Trace
→ 写入 Eval 结果
→ 返回答案
```

你需要实现的 Harness 模块：

### AgentState

记录一次任务的状态：

```python
class AgentState:
    query: str
    steps: list
    retrieved_docs: list
    final_answer: str
    errors: list
    status: str
```

### ToolResponse

统一工具返回：

```json
{
  "status": "success | partial | error",
  "text": "给 LLM 或用户阅读的文本",
  "data": {},
  "error": {},
  "stats": {
    "time_ms": 123
  }
}
```

### ToolRegistry

统一注册和调用工具：

```python
registry.register("search_docs", search_docs)
registry.call("search_docs", {"query": "...", "top_k": 5})
```

### TraceLogger

每一步写入 JSONL：

```json
{"step": 1, "tool": "search_docs", "input": "...", "output": "...", "time_ms": 120}
```

### EvalRunner

准备 `eval_questions.jsonl`：

```json
{"question": "RAG 的核心流程是什么？", "gold_answer": "文档切分、向量化、检索、生成", "gold_doc": "rag_intro"}
```

统计：

- retrieval hit rate
- answer correctness
- citation accuracy
- latency
- tool success rate

### Guard

控制工具权限：

- 只允许调用白名单工具
- 不允许任意执行 shell
- 不允许随意写文件
- 设置最大执行步数
- 设置超时和重试次数

## 6. 一个月安排

### 第 1 周：RAG 最小闭环

主学：[all-in-rag](https://github.com/datawhalechina/all-in-rag)

目标：

- 跑通一个最小 RAG
- 能对本地文档问答
- 答案带引用来源

任务：

- Day 1：准备 Python 环境、Git、API Key
- Day 2：文档加载，支持 Markdown / TXT / PDF
- Day 3：文本切分，比较 chunk size 和 overlap
- Day 4：Embedding + FAISS / Chroma
- Day 5：Top-K 检索 + Prompt 组装
- Day 6：答案生成 + 来源引用
- Day 7：写 README 和运行示例

穿插学习 happy-llm：

- Tokenizer
- Embedding
- Transformer 概览
- 推理参数 temperature / top-p / max tokens

### 第 2 周：RAG 优化与评估

主学：all-in-rag

目标：

- 加入 BM25 / Hybrid Search / Rerank
- 建立小型评测集

任务：

- 准备 20～30 条评测问题
- 比较向量检索、BM25、Hybrid Search
- 加入 Rerank
- 统计召回命中率、答案正确率、引用准确率
- 记录失败案例

产出：

- `eval_questions.jsonl`
- `eval_runner.py`
- RAG v2 README

### 第 3 周：Agent + Harness

主学：[hello-agents](https://github.com/datawhalechina/hello-agents)

目标：

- 把 RAG 流程封装成工具
- 实现一个简单 Agent Runtime
- 加入 Trace 和 Step Limit

任务：

- 实现 `ToolResponse`
- 实现 `ToolRegistry`
- 实现 `AgentState`
- 把 `search_docs`、`rerank_docs`、`generate_answer` 封装成工具
- 实现 `TraceLogger`
- 设置最大执行步数和失败重试

产出：

- Mini RAG Agent Harness
- JSONL Trace
- 工具调用日志

### 第 4 周：LoRA 加分实验 + 项目包装

主学：[self-llm](https://github.com/datawhalechina/self-llm)

目标：

- 跑通一个小规模开源模型推理或 LoRA 实验
- 完成最终项目文档和面试表达

任务：

- 用 Transformers 加载 Qwen / GLM / MiniCPM 小模型
- 准备 50～200 条小数据
- 跑通 LoRA / QLoRA 微调流程
- 对比 Base / RAG / LoRA / RAG + LoRA
- 整理 README、架构图、实验结果、失败案例

如果没有 GPU：

- 主项目继续使用 API
- LoRA 部分写成“短租 GPU 实验”或“流程复现”
- 至少掌握数据格式、PEFT 配置、训练参数

## 7. 服务器和显存建议

### 调用 API

不需要自己的 GPU。

适合：

- RAG
- Agent
- Harness
- Tool Calling
- Eval
- Web Demo

配置：

```text
CPU：4 核以上
内存：16GB 推荐
硬盘：50GB+
GPU：不需要
```

### 本地模型推理 / 微调

需要 GPU，尤其是 self-llm 部分。

大致显存：

| 任务 | 建议显存 |
|---|---:|
| 本地 Embedding 小模型 | 4GB～8GB |
| 1.5B 模型推理 | 4GB～8GB |
| 3B 模型推理 | 8GB～12GB |
| 7B / 8B 量化推理 | 12GB～16GB |
| 7B / 8B FP16 推理 | 16GB～24GB |
| 7B / 8B QLoRA 微调 | 24GB 推荐 |
| 14B 量化推理 | 24GB 左右 |
| 14B QLoRA 微调 | 48GB 推荐 |
| 32B 推理 / 微调 | 48GB～80GB+ |

推荐短租：

```text
RTX 4090 24GB / A10 24GB
CPU 8 核
内存 32GB～64GB
硬盘 100GB～200GB
Ubuntu 22.04
```

最省钱策略：

```text
主项目：本地电脑 + API
加分实验：短租 24GB 显存机器跑一次 LoRA
最终部署：仍然用 API 版本
```

## 8. 简历项目描述模板

可以写成：

> 基于 RAG + Agent Harness 的领域知识问答系统  
> 使用 Python、FAISS/Chroma、Embedding、BM25/Hybrid Search、Rerank、LLM API 构建知识库问答系统。将检索、重排、答案生成封装为标准工具，设计 ToolResponse、AgentState、ToolRegistry、TraceLogger 和 EvalRunner，实现可观测、可评估、可恢复的 RAG Agent。构建 30 条评测问题，对比 Base LLM、RAG、RAG + Rerank、RAG Agent 的召回率、引用准确率、答案正确率和延迟。

如果完成 LoRA 实验，可以补一句：

> 额外使用 self-llm 路线复现 Qwen 小模型 LoRA/QLoRA 微调流程，对比 Base、LoRA、RAG、RAG+LoRA 在领域问答上的表现。

## 9. 面试重点问题

至少准备这些：

1. RAG 的完整流程是什么？
2. Chunk size 太大或太小有什么问题？
3. Embedding 和 BM25 有什么区别？
4. 为什么需要 Hybrid Search？
5. Rerank 的作用是什么？
6. RAG 为什么仍然可能幻觉？
7. 如何评估 RAG 系统？
8. Agent 和普通 Chatbot 有什么区别？
9. Tool Calling 怎么保证参数正确？
10. Harness 是什么？
11. 为什么 Agent 需要状态管理？
12. 如何避免 Agent 无限循环？
13. TraceLogger 记录什么？
14. EvalRunner 怎么设计？
15. LoRA 和全参数微调有什么区别？
16. QLoRA 为什么省显存？
17. 本地跑 7B 模型大概需要多少显存？
18. 调用 API 和本地部署模型有什么差别？
19. 你的项目如何降低幻觉？
20. 你的项目还有哪些可优化方向？

## 10. 最终交付物

1 个月后至少应该有：

- 一个 `Mini RAG Agent Harness` 项目
- 一个完整 README
- 一个可运行 Demo
- 一份评测集
- 一份评测结果
- 一份 Trace 日志样例
- 一个 LoRA / 本地模型实验记录
- 一份面试问题笔记

最终目标不是“什么都学完”，而是能清楚讲出：

```text
我理解大模型原理，也能把 RAG / Agent 系统做成可观测、可评估、可恢复的工程项目。
```
