---
title: "大语言模型（LLM）"
type: concept
sources: ["deep-learning-nlp.md"]
created: 2026-06-06
updated: 2026-06-06
---

# 大语言模型（Large Language Model, LLM）

> 基于 Transformer 的大规模预训练语言模型，通常具有数十亿至数千亿参数。

## 涌现能力（Emergent Abilities）

- **上下文学习（In-context Learning）**：模型能通过 prompt 中的示例学习新任务
- **思维链推理（Chain-of-Thought）**：逐步推理提升复杂任务准确率
- **指令遵循（Instruction Following）**：遵循人类指令完成多样化任务

## 典型模型

- **GPT 系列**：OpenAI
- **LLaMA 系列**：Meta AI
- **通义千问**：阿里巴巴
- **DeepSeek-V3/R1**：DeepSeek
- **Gemini**：Google DeepMind
- **Claude**：Anthropic

## 训练流程

1. 预训练：海量文本上自监督学习
2. 微调：指令数据微调
3. 对齐：RLHF/DPO 使行为符合人类偏好

## 相关页面
- [[transformer]]（基础架构）
- [[attention-mechanism]]（核心组件）
- [[alignment]]（对齐技术）
- [[retrieval-augmented-generation]]（RAG 增强方案）
