# Schema

本文件定义 Wiki 的结构规则和命名约定，由 LLM 严格遵守。

## 目录结构

```
wiki/
├── index.md        ← 内容目录（LLM 每次摄入后更新）
├── log.md          ← 时序操作记录（append-only）
├── overview.md     ← 全局概要（自动更新）
├── schema.md       ← 本文件
├── purpose.md      ← 目标定位
├── entities/       ← 人物、组织、产品
├── concepts/       ← 理论、方法、技术
├── sources/        ← 资料摘要（每份原始资料对应一个）
├── queries/        ← 保存的聊天回答与研究
└── comparisons/    ← 并列对比表

raw/
└── sources/        ← 原始资料（不可变，仅读取）
```

## 命名约定
- 文件名使用 **KebabCase**：`machine-learning.md`、`alexander-dugin.md`
- 实体命名：`{实体名}.md`，如 `openai.md`、`gpt-4.md`
- 概念命名：`{概念名}.md`，如 `reinforcement-learning.md`
- 文件夹层级最多 2 层

## 页面 Frontmatter（必需）
所有生成的 Wiki 页面必须包含 YAML frontmatter：

```yaml
---
title: "页面标题"
type: [entity|concept|source|synthesis|query|comparison]
sources: ["原始文件名1.md", "原始文件名2.md"]
created: 2026-06-06
updated: 2026-06-06
---
```

- `sources[]`：对应该页面涉及的全部原始资料文件名（供溯源）
- `type`：必填，枚举值见下

## 页面类型
| 类型 | 用途 | 文件夹 |
|------|------|--------|
| `entity` | 人物、组织、产品、项目 | `entities/` |
| `concept` | 理论、方法、技术术语 | `concepts/` |
| `source` | 单份原始资料的摘要 | `sources/` |
| `synthesis` | 跨来源综合分析 | 根目录或 `synthesis/` |
| `query` | 保存的聊天回答 | `queries/` |
| `comparison` | 并列对比表格 | `comparisons/` |

## 交叉引用
- 使用 **Obsidian wikilinks**：`[[实体名]]`、`[[概念名]]`
- 实体页面之间互相引用时，放在 "相关概念" 部分
- 概念页面引用来源时，链接到 `[[source/文件名]]`

## 更新规则
- **增量更新**：不重写整页，只追加、修正、补充
- **来源追溯**：每次更新必须维护 `sources[]`，新增来源时追加数组
- **矛盾标记**：若新来源与已有页面冲突，在页面底部追加 "## 待审核" 并简述冲突点

## 日志格式
`log.md` 每条记录以时间戳开头，可被 unix 工具解析：

```markdown
## [YYYY-MM-DD HH:MM] ingest | 原始文件名.md
- 新增页面：entities/xxx.md, concepts/xxx.md
- 更新页面：index.md, overview.md
- 审核项：0
```

## 禁止操作
- 禁止修改 `raw/sources/` 下的任何文件
- 禁止删除 `sources[]` 中的条目（标记为过时代替删除）
- 禁止创建不属于上述类型的新文件夹（如需扩展请先修改本 schema 并记录）
