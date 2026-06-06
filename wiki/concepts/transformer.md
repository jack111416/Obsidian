---
title: "Transformer 架构"
type: concept
sources: ["deep-learning-nlp.md"]
created: 2026-06-06
updated: 2026-06-06
---

# Transformer 架构

> 2017 年 Google Brain 提出的神经网络架构，彻底改变了 NLP 和 CV 领域。

## 核心机制

### 自注意力（Self-Attention）
- 计算序列中每个位置与其他位置的关联权重
- 允许并行处理，适合 GPU 加速

### 位置编码（Positional Encoding）
- 由于注意力本身没有位置信息，需额外注入

### 前馈网络 + 残差连接
- 每层包含多头注意力和全连接前馈网络

## 关键变体

| 变体 | 作者/机构 | 特点 |
|------|----------|------|
| Encoder（BERT） | Google | 双向编码，适合理解任务 |
| Decoder（GPT） | OpenAI | 单向自回归，适合生成 |
| Encoder-Decoder（T5） | Google | 适合 seq2seq 任务 |

## 影响

- 成为现代 LLM（GPT、LLaMA、Qwen 等）的基础
- 被引入 Vision Transformer（ViT），扩展至 CV

## 相关页面
- [[google-brain-deepmind]]（提出者）
- [[attention-mechanism]]（核心组件）
- [[large-language-model]]（应用）
