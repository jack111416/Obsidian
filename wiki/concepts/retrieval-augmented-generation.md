---
title: "检索增强生成（RAG）"
type: concept
sources: ["deep-learning-nlp.md"]
created: 2026-06-06
updated: 2026-06-06
---

# 检索增强生成（RAG）

> 通过外部知识库动态增强生成质量，减少 LLM 的幻觉问题。

## 工作原理

```
Query → 检索相关文档 → 拼接到 Prompt → LLM 生成带引用的回答
```

## 优势

- 无需重新训练模型即可更新知识
- 可追溯引用来源
- 减少事实性错误（Hallucination）

## 与纯 LLM 对比

| 维度 | 纯 LLM | RAG |
|------|--------|-----|
| 知识更新 | 需微调或重新训练 | 即时更新知识库 |
| 可追溯 | 困难 | 直接引用来源 |
| 幻觉 | 较高 | 显著降低 |
| 成本 | 高 | 低 |

## 相关页面
- [[large-language-model]]（RAG 依赖 LLM）
- [[alignment]]（对齐相关）
