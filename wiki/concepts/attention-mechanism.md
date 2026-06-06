---
title: "注意力机制（Attention）"
type: concept
sources: ["deep-learning-nlp.md"]
created: 2026-06-06
updated: 2026-06-06
---

# 注意力机制（Attention）

> 让模型在生成每个位置时，能动态关注输入序列的相关部分。

## 发展历程

- **2014**：Bahdanau Attention（机器翻译，Seq2Seq with Attention）
- **2015**：Luong Attention（多种注意力评分函数）
- **2017**：**自注意力（Self-Attention）** → Transformer

## 核心公式

```
Attention(Q, K, V) = softmax(QK^T / √d_k) · V
```

- Q（Query）、K（Key）、V（Value）由输入线性变换得到
- 缩放因子防止梯度消失

## 多头注意力（Multi-Head Attention）

- 多个注意力头并行计算
- 允许模型关注不同子空间的信息

## 相关页面
- [[transformer]]（直接构成组件）
- [[large-language-model]]（应用）
