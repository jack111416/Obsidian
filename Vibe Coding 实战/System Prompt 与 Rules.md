# 前言

> **本讲目标：** 学会用规则稳定 AI 的输出风格，在 CodeBuddy 中建立一套个性化编码规范，让生成的代码更贴近你的习惯。
> 
> **预计时长：** 60 分钟
> 
> **难度等级：** 入门

## 自学导航卡

- **前置依赖：** 已完成第 1 讲，能在项目中创建文件与文件夹（无需编程基础）
- **预计耗时：** 新手 60-90 分钟｜有经验 45-60 分钟
- **完成标准：** 建立 10+ 条可验证规则，并验证同需求两次生成风格趋同
- **卡点入口：** 若规则不生效，先检查路径与“必须/禁止”约束写法

# 核心概念：什么是 System Prompt？

## 一、理解 AI 的"性格设定"

每一个 AI 对话都有一个隐藏的"人设说明书"，这就是 **System Prompt（系统提示词）**。

![](https://vscode-remote+d7b1ad34100244f9984d87fdbe44ff40-002eap-002dshanghai2-002ecloudstudio-002eclub.vscode-resource.vscode-cdn.net/workspace/.tutorial/images/2-1.png)

**System Prompt 决定了 AI 的：**

- 编码风格偏好（函数式 vs 面向对象）
- 语言/框架选择（TypeScript vs JavaScript）
- 注释习惯（中文 vs 英文、详细 vs 精简）
- 错误处理策略（静默处理 vs 抛出异常）
- 代码组织方式（单文件 vs 模块拆分）

## 二、为什么需要定制 System Prompt？

不定制 System Prompt = 每次都和一个"陌生人"合作：

|场景|不定制|定制后|
|---|---|---|
|变量命名|可能用 `data`/`info`/`result`|统一用你喜欢的命名规范|
|类型系统|可能给你 JS 也可能给 TS|强制使用 TypeScript + 严格模式|
|样式方案|可能用 CSS Module、Styled 等|统一用 TailwindCSS|
|注释语言|有时中文有时英文|统一中文注释|
|错误处理|可能忽略边界情况|强制处理所有异常|