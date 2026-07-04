# Huashu-Design · 项目交付笔记

> **创建：** 2026-06-08  
> **状态：** 进行中（7 页 deck 已完成，待用户最终确认）  
> **路径：** `~/.hermes/skills/huashu-design/projects/huashu-report/`

---

## 一、项目是什么

**Huashu-Design** 是花叔设计团队维护的一套「用 HTML 做高保真设计产出」的 skill / 工作流。

核心命题：**品牌识别度 > 视觉安全区**。反对 AI 生成的「视觉最小公约数」（统一深蓝底 + 紫色霓虹 + 圆角卡片 + 无意义渐变）。

**技术栈：**  
- 每页独立 HTML（1920×1080）  
- `shared/tokens.css` 全局设计 token  
- `assets/deck_index.html` 多文件拼接器（双概览模式：网格墙 + 无限画廊）  
- 导出：`scripts/export_deck_pdf.mjs` / `scripts/export_deck_pptx.mjs`  

**部署方式：**  
- HTTP server：`python3 -m http.server 8899 --bind 0.0.0.0`（项目根目录）  
- 同一 WiFi 访问：`http://192.168.31.68:8899/`（Termux / Android）  

---

## 二、这次交付了什么

**项目路径：** `~/.hermes/skills/huashu-design/projects/huashu-report/`

```
huashu-report/
├── index.html          ← deck 拼接器（MANIFEST 已注册 7 页）
├── shared/
│   └── tokens.css      ← 全局设计 token
└── slides/
    ├── 01-cover.html
    ├── 02-background.html
    ├── 03-core-features.html
    ├── 04-mechanisms.html
    ├── 05-brand-asset-protocol.html
    ├── 06-aura-showcase.html
    └── 07-tech-future.html
```

**7 页结构：**

| 页 | 标题 | 布局特点 |
|----|------|----------|
| 01 | Cover | 大字 + 右侧红框（DESIGN SKILL / HTML-FIRST / V1.0）|
| 02 | Background | 双栏：左侧叙事 + 右侧数据卡片 |
| 03 | Core Features | 三列卡片（Anti-Slop / Asset-First / HTML-First）|
| 04 | Mechanisms | 上下分割，左标题右详情（并行机制）|
| 05 | Brand Asset Protocol | 五步 pipeline 横向流程图 |
| 06 | Aura Showcase | 双栏产品卡 + 设计方向卡 |
| 07 | Tech & Future | 左侧堆栈时间轴 + 右侧结尾大字 |

**设计系统（tokens.css）：**

```
--bg:        #0a0908  暖黑底
--surface:   #141210  卡片底
--border:    #2a2420  边框
--copper:    #c8895a  主 accent
--copper2:   #e2a374  浅 accent
--text:      #faf8f5  正文白
--muted:     #c4bfb9  辅助灰
--muted2:    #8a8580  弱化灰
--serif:     Georgia / Songti SC / Noto Serif SC
--sans:      -apple-system / PingFang SC / Microsoft YaHei
```

**字号最终状态：**

| 类型 | 值 | 说明 |
|------|-----|------|
| 正文 `.body-text` | 24px (1.5rem) | 修正 deck fit 缩放补偿 |
| 小节副标题 `.slide-subtitle` | 25px (1.6rem) | 斜体，不变 |
| 内容页大标题 | clamp(35px–58px) | 保留弹性 |
| 封面主标题 | clamp(51px–104px) | 保留弹性 |
| kicker / label | 15px | 章节标签 |
| 数据大数字 | 48px | 统计页 |

---

## 三、关键决策与踩坑记录

1. **HTML-first 强约束**：PDF / PPTX 是衍生物，永远先做 HTML deck，再按需导出。
2. **多文件架构**：每页独立 HTML + `deck_index.html` 拼接器，避免单文件 CSS 污染。
3. **封面居中**：父级 `.slide` flex 容器干扰 → 最终用 `position:absolute; top:50%; left:50%; transform:translate(-50%,-50%)`。
4. **红框清晰化**：原 `#c8895a` 铜橙与主题撞色 → 改为 `#b83a2a` 深红 + 白字加粗 + 独立浅橙版本号。
5. **字号修改反复失效**：tokens.css 改完后不生效 → 发现每页内联 CSS 覆盖 → 需逐页同步修改内联规则。
6. **Deck fit 缩放**：单页直开 vs 聚合页全屏演示有感知差异 → 原因：iframe fit 等比缩放 → 补偿方案：正文提到 24px。
7. **文件同步 bug**：早期编辑 `report-showcase.html` 但服务跑的是 `index.html` → 铁律：永远直接编辑 `ppts/index.html`（本项目已转型多文件，改为直接编辑 `slides/*.html`）。

---

## 四、下一步（开放循环）

- [ ] 用户确认 7 页 deck 视觉 ok
- [ ] 如需 PDF：`node scripts/export_deck_pdf.mjs`（需先装 playwright + pdf-lib）
- [ ] 如需可编辑 PPTX：`node scripts/export_deck_pptx.mjs`（需按 4 条硬约束写 HTML，本项目暂未触发）
- [ ] 考虑用 `scripts/gen_deck_thumbs.mjs` 生成缩略图优化概览墙加载
- [ ] 如内容有更新，只改 `slides/*.html`，`shared/tokens.css` 是设计系统唯一入口

---

## 五、相关链接

- [[huashu-design SKILL]] → skill 本体
- [[Huashu-Design · 项目交付笔记]] ← 本页
- `~/.hermes/skills/huashu-design/projects/huashu-report/` → 项目目录
