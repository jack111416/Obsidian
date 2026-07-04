# CSS Token 优先级陷阱（多文件架构，2026-06-08 实测）

**症状**：修改了 `shared/tokens.css` 的全局 token（如 `.body-text { font-size: ... }`），但前台页面没有任何变化。

**根因**：多文件架构中，每页 slide 的内联 `<style>` 块也定义了相同 class 的规则。浏览器中，内联样式与外部 `tokens.css` 优先级相同，但靠文档顺序**后者覆盖前者**——如果内联 `<style>` 在 `<link>` 之后（正常情况），单页的规则会赢。结果：全局 token 被单页内联规则覆盖，改全局等于没改。

**强制验收流程**（改完全局 token 后必须执行）：
1. 全局调完 `shared/tokens.css` 后，立刻在项目根跑：
   ```bash
   grep -h "font-size" slides/*.html | grep -E "body-text|slide-subtitle|mech-desc|showcase-body|stack-desc|step-desc"
   ```
2. 发现任一页还为旧值 → **逐页 patch 该 slide**，不要只依赖全局
3. 页数 > 5 时用批量替换加速：
   ```bash
   sed -i 's/font-size: 0\.875rem;/font-size: 1rem;/g' slides/*.html
   ```
4. **从 HTTP 服务侧验证，不是只查本地文件**：
   ```bash
   curl -s http://localhost:PORT/slides/NN-name.html | grep -E "body-text|font-size"
   ```
5. 最终让用户强制刷新浏览器（Cmd+Shift+R / 清缓存后重载）

**复盘**：在 `projects/huashu-report` 里连续 3 次出现「改了 tokens.css 但前台没变化」，根因就是 7 页 slide 里有 5 页各自写了 `font-size: 0.875rem` 覆盖了全局的 `1rem`。教训：**改全局 token 的同时必须 grep 全量单页内联规则，否则 90% 的情况等于白改**。

## 暗色主题 Deck 设计建议

### 封面红框可读性铁律

暗底封面上的彩色信息框（红/橙/铜），框内文字必须：
1. **对比度 ≥ 4.5:1**（WCAG AA）—— 框本身不能和背景太接近
2. **字重 ≥ 700** —— 小字号在深色框上显得更淡，必须用粗体补偿
3. **字号 ≥ 16px（1rem）** —— 小于这个在移动端基本无法阅读
4. **版本号单独上浅亮色**（如 `#ffe8d6`），和普通文字拉开层次

### 正文字号下限参考

暗底 + 低对比文字组合下：
- 正文最小：`1rem`（16px），行高 ≥ 1.75
- 小节副标题：`1.25rem`（20px）起
- 内容页大标题：`clamp(2.2rem, 4.5vw, 3.6rem)`
- 封面主标题：`clamp(3.2rem, 7.5vw, 6.5rem)`

### 多文件架构快速参考

```
项目根/
├── index.html        ← 从 assets/deck_index.html 复制，改 MANIFEST
├── shared/
│   └── tokens.css    ← 全局 token（色板/字号/布局）
└── slides/
    ├── 01-cover.html
    ├── 02-xxx.html
    └── ...
```

**每页 slide 的内联 CSS 会覆盖 tokens.css 的同名 class** — 改 token 后务必 grep 确认。
