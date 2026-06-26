# 大模型算法/开发岗位 1 个月入门学习计划

> 适用对象：只有一点 Python 基础，目标是在 1 个月内建立大模型算法/开发岗位的知识框架，完成可展示的小项目，并能开始投递实习/初级岗位。
>
> 重要预期：1 个月很难达到“独立做大模型算法研究”的水平，但可以完成“能看懂主流 LLM 应用开发流程、能做 RAG/Agent/微调 Demo、能讲清 Transformer 与训练推理基础、能准备简历项目”的阶段目标。

## 1. 岗位方向先分清

大模型相关岗位通常可以分为两类，建议先以“大模型应用开发”为主线，再补算法基础。

### 1.1 大模型应用开发

主要做：

- 调用大模型 API 或本地开源模型。
- 构建 RAG 知识库问答、Agent、工作流、聊天机器人。
- 处理 Prompt、向量数据库、后端接口、评测、部署。

你需要优先掌握：

- Python 基础与工程能力。
- OpenAI/通义/智谱/DeepSeek/本地模型 API 调用方式。
- Prompt Engineering。
- Embedding、向量数据库、RAG。
- LangChain 或 LlamaIndex 等框架。
- FastAPI、简单前端或命令行 Demo。

### 1.2 大模型算法/训练方向

主要做：

- 模型结构、训练、微调、推理优化。
- 数据清洗、SFT、LoRA/QLoRA、评测。
- 阅读论文、复现实验、调参。

你需要逐步掌握：

- 机器学习、深度学习基础。
- PyTorch。
- Transformer、Attention、Tokenizer。
- Hugging Face Transformers/Datasets/PEFT。
- 微调、量化、推理加速基础。

如果你目前只有一点 Python 基础，建议 1 个月内不要平均用力，而是采用：

> 70% 时间做大模型应用开发项目，30% 时间补算法与训练基础。

## 2. 一个月总目标

一个月结束时，建议至少完成以下成果：

1. 一个 GitHub 项目：基于本地资料或网页资料的 RAG 知识库问答系统。
2. 一个微调 Demo：用小数据对开源模型做 LoRA/QLoRA 微调，能说明流程即可。
3. 一份学习笔记：解释 Transformer、Attention、Embedding、RAG、微调、Agent。
4. 一份简历项目描述：能讲清需求、架构、技术选型、难点、改进方向。

## 3. 每日学习节奏建议

如果每天可以学习 4-6 小时，建议这样分配：

- 1 小时：Python/算法/深度学习基础。
- 1 小时：大模型理论或文档阅读。
- 2-3 小时：项目实践。
- 30 分钟：总结笔记与复盘。

如果每天只有 2-3 小时，优先级调整为：

1. 项目实践。
2. 大模型应用核心概念。
3. Python 与 PyTorch 基础。
4. 算法论文与细节。

## 4. 第 1 周：Python 工程基础 + LLM 基础认知

### 目标

- 能熟练写 Python 脚本。
- 能调用一个大模型 API 完成问答。
- 理解大模型应用开发的基本链路。

### 学习内容

#### Python 必会内容

- 变量、函数、类、异常、文件读写。
- list、dict、set、tuple。
- requests/httpx 调接口。
- argparse 或 typer 写命令行脚本。
- venv/conda、pip、requirements.txt。
- Git 基础：clone、commit、branch、push。

#### 大模型基础概念

- 什么是 Token、Tokenizer、上下文窗口。
- 什么是 Prompt、System Prompt、Few-shot。
- 什么是 Temperature、Top-p、Max Tokens。
- 什么是 Embedding。
- 什么是 RAG。

### 实践任务

1. 写一个 `chat_cli.py`：命令行输入问题，调用大模型 API 返回答案。
2. 写一个 `summarize_file.py`：读取本地 Markdown/TXT 文件，让模型总结内容。
3. 记录 10 个 Prompt 示例，比较不同写法的效果。

### 第 1 周验收标准

- 能解释一次 API 请求包括哪些字段。
- 能独立写一个简单聊天脚本。
- 能说清 Prompt、Token、Embedding、RAG 的作用。

## 5. 第 2 周：RAG 知识库问答项目

### 目标

- 完成一个最小可用的知识库问答系统。
- 理解文档切分、向量化、召回、重排、生成的流程。

### 学习内容

- 文档加载：txt、md、pdf。
- 文档切分：chunk size、chunk overlap。
- Embedding 模型。
- 向量数据库：Chroma、FAISS、Milvus 三选一，入门建议 Chroma 或 FAISS。
- RAG 基本流程：检索相关片段，再交给 LLM 生成答案。
- 简单评测：答案是否引用到正确资料、是否幻觉。

### 实践任务

做一个“本地资料问答助手”：

1. 准备 `docs/` 目录，放入你的学习资料。
2. 写入库脚本：读取资料、切分、生成 embedding、存入向量库。
3. 写问答脚本：输入问题、检索 Top-K 文档片段、拼 Prompt、生成答案。
4. 输出答案时附带来源片段。

### 推荐项目结构

```text
llm-rag-assistant/
├── README.md
├── requirements.txt
├── docs/
├── src/
│   ├── ingest.py
│   ├── query.py
│   ├── llm.py
│   └── retriever.py
└── data/
```

### 第 2 周验收标准

- 能对自己的资料进行问答。
- 能解释为什么需要切分文档。
- 能解释 Top-K、Embedding、向量相似度。
- README 中有安装、运行、效果截图或示例。

## 6. 第 3 周：Transformer/PyTorch/Hugging Face/微调入门

### 目标

- 理解 Transformer 的核心结构。
- 能使用 Hugging Face 加载模型与 tokenizer。
- 能跑通一个小规模 LoRA 微调流程。

### 学习内容

#### PyTorch 基础

- Tensor。
- Dataset、DataLoader。
- nn.Module。
- loss、optimizer、反向传播。
- GPU/CUDA 基础概念。

#### Transformer 基础

重点理解，不要求手写完整大模型：

- Tokenization。
- Embedding。
- Positional Encoding。
- Self-Attention。
- Multi-Head Attention。
- Feed Forward Network。
- LayerNorm、Residual Connection。
- Decoder-only 架构。

#### Hugging Face 生态

- transformers。
- datasets。
- tokenizers。
- accelerate。
- peft。
- bitsandbytes。

### 实践任务

1. 用 Hugging Face 加载一个小模型，完成文本生成。
2. 准备一个很小的指令数据集，例如 50-200 条问答。
3. 使用 PEFT 跑通 LoRA 微调。
4. 对比微调前后模型输出变化。

### 第 3 周验收标准

- 能说清 Transformer 为什么适合处理语言序列。
- 能解释 Attention 在做什么。
- 能跑通模型加载、推理、LoRA 微调的完整流程。
- 能说明 SFT、LoRA、QLoRA 的区别。

## 7. 第 4 周：Agent/评测/部署/求职准备

### 目标

- 给 RAG 项目增加工程化能力。
- 完成简历项目包装。
- 准备面试问答。

### 学习内容

#### Agent 与工具调用

- Function Calling / Tool Calling。
- ReAct 思路。
- 工具：搜索、计算器、数据库查询、本地文件查询。
- Agent 的边界：不稳定、成本高、需要评测和约束。

#### 评测与优化

- 检索准确率。
- 答案相关性。
- 幻觉率。
- Prompt A/B 测试。
- 日志记录。

#### 部署

- FastAPI 提供后端接口。
- Streamlit 或 Gradio 做简单页面。
- Docker 基础。
- 环境变量管理 API Key。

### 实践任务

在第 2 周 RAG 项目基础上增加：

1. Web 页面：用 Streamlit 或 Gradio。
2. API 服务：用 FastAPI 暴露 `/chat` 接口。
3. 日志：记录问题、召回片段、回答、耗时。
4. 评测集：准备 20 个问题，记录回答是否正确。
5. README：补充项目背景、架构图、运行方式、效果展示、未来优化。

### 第 4 周验收标准

- 有一个能演示的 RAG Web Demo。
- 有一个可以写进简历的项目。
- 能讲清项目架构与核心技术点。
- 准备好 15-20 个常见面试问题答案。

## 8. 30 天具体安排

| 天数 | 重点任务 | 产出 |
| --- | --- | --- |
| Day 1 | Python 环境、Git、虚拟环境、pip | 能创建项目并安装依赖 |
| Day 2 | Python 文件读写、requests/httpx | 文件总结脚本 |
| Day 3 | LLM API 参数、Prompt 基础 | chat_cli.py |
| Day 4 | Prompt 模板、Few-shot | Prompt 对比笔记 |
| Day 5 | Token、上下文窗口、费用估算 | LLM 基础笔记 |
| Day 6 | 整理第 1 周脚本 | 小型 CLI Demo |
| Day 7 | 复盘与补缺 | 第 1 周总结 |
| Day 8 | RAG 原理、Embedding | RAG 流程图 |
| Day 9 | 文档加载与切分 | ingest.py 初版 |
| Day 10 | Chroma/FAISS 入门 | 本地向量库 |
| Day 11 | 检索 Top-K 文档 | retriever.py |
| Day 12 | 拼接 Prompt 生成答案 | query.py |
| Day 13 | 来源引用与回答优化 | 可用 RAG CLI |
| Day 14 | README 与示例整理 | RAG 项目 v1 |
| Day 15 | PyTorch Tensor、Dataset | PyTorch 练习 |
| Day 16 | nn.Module、训练循环 | 简单分类模型 |
| Day 17 | Tokenizer、Embedding | Tokenizer 笔记 |
| Day 18 | Attention、Transformer | Transformer 笔记 |
| Day 19 | Hugging Face 推理 | 本地模型推理脚本 |
| Day 20 | LoRA/PEFT 入门 | 微调脚本初版 |
| Day 21 | 微调结果对比 | 微调 Demo |
| Day 22 | Agent 与工具调用 | 简单工具调用 Demo |
| Day 23 | FastAPI | `/chat` 接口 |
| Day 24 | Streamlit/Gradio | Web Demo |
| Day 25 | 日志与配置管理 | 项目工程化 |
| Day 26 | 准备 20 条评测问题 | eval.json |
| Day 27 | 跑评测并分析问题 | 评测记录 |
| Day 28 | 优化 Prompt/切分/Top-K | RAG 项目 v2 |
| Day 29 | 简历项目描述 | 简历项目文案 |
| Day 30 | 模拟面试与复盘 | 面试题答案清单 |

## 9. 必学技术清单

### 第一优先级

- Python。
- Git。
- LLM API 调用。
- Prompt Engineering。
- Embedding。
- RAG。
- 向量数据库。
- FastAPI 或 Streamlit/Gradio。

### 第二优先级

- PyTorch。
- Transformer。
- Hugging Face Transformers。
- LoRA/QLoRA。
- PEFT。
- 模型评测。

### 第三优先级

- Docker。
- Linux 基础。
- CUDA/GPU 环境。
- 分布式训练。
- 推理优化。

## 10. 推荐学习资料类型

你已经有资料时，建议按下面顺序消化，不要一开始就啃论文：

1. Python 项目实战教程。
2. 大模型 API 官方文档。
3. RAG 项目教程。
4. Hugging Face 官方课程。
5. PyTorch 官方入门教程。
6. Transformer 可视化讲解。
7. LoRA/QLoRA 实践文章。
8. Attention Is All You Need 论文精读。

## 11. 面试需要会讲的问题

一个月后至少准备这些问题：

1. 大模型一次推理请求的流程是什么？
2. Prompt Engineering 有哪些常见技巧？
3. Token 和上下文窗口是什么？
4. Embedding 是什么，为什么能做语义检索？
5. RAG 的完整流程是什么？
6. RAG 相比直接把资料塞进 Prompt 有什么优势？
7. RAG 常见问题有哪些？如何优化？
8. 向量数据库的作用是什么？
9. Top-K 检索是什么意思？
10. Transformer 的核心结构是什么？
11. Self-Attention 在做什么？
12. Decoder-only 模型为什么适合文本生成？
13. SFT、LoRA、QLoRA 分别是什么？
14. 如何评测一个问答系统？
15. 你的项目架构是什么？
16. 你的项目有哪些难点？
17. 如果用户问的问题检索不到资料，你怎么处理？
18. 如何降低幻觉？
19. 如何控制 API 成本？
20. 如何把 Demo 部署成服务？

## 12. 简历项目模板

可以把最终项目包装成下面这种描述：

> 基于大语言模型的本地知识库问答系统  
> 使用 Python、LangChain/LlamaIndex、Chroma/FAISS、FastAPI、Streamlit 构建企业/个人文档问答系统。实现文档解析、文本切分、Embedding 向量化、Top-K 语义检索、Prompt 组装、LLM 生成与来源引用。设计 20 条评测问题对系统进行效果评估，并通过调整 chunk size、Top-K 和 Prompt 模板降低幻觉、提升回答相关性。

可以拆成技术点：

- 负责文档入库流程，实现 Markdown/TXT/PDF 文档解析、切分与向量化。
- 基于向量数据库实现语义检索，并将召回片段作为上下文输入 LLM。
- 使用 FastAPI 封装问答接口，使用 Streamlit/Gradio 搭建演示页面。
- 构建小型评测集，记录召回片段、回答质量与耗时，持续优化 Prompt 和检索参数。

## 13. 学习注意事项

- 不要只看视频，一定每天写代码。
- 不要一开始追求训练大模型，先把 RAG 和 API 应用做出来。
- 不要沉迷框架，先理解“检索 + 生成”的本质流程。
- 不要把所有概念都学到 100% 再动手，先做最小 Demo。
- 每天都写学习日志，面试时这些日志会帮你组织语言。

## 14. 最终交付物清单

一个月结束时，建议你至少拥有：

- `llm-rag-assistant` 项目代码。
- `README.md` 项目说明。
- 5-10 张运行截图或终端输出示例。
- 20 条评测问题与评测结果。
- 1 个 LoRA 微调 Demo。
- 1 份大模型基础知识笔记。
- 1 份简历项目描述。
- 1 份面试问答清单。
