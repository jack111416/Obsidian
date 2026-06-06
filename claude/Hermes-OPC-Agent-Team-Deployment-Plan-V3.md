# OPC Agent 团队部署实施计划（V3 — 终极整合版）

> **2026-06-04**。整合三个项目：
> - **Hermes** 多 Profile 协作 + Wiki 共享记忆（组织架构）
> - **OPC-Agents** 三贤者引擎 + 21 技能 + CarryMem（执行引擎）
> - **OPC Team** 20 角色目录 + 三档弹性编组 + 任务状态机 + 决策履历 + 风险量化 + 三级记忆 + 多平台适配（治理框架）

---

## 三项目职责划分（不再重复的部分）

| 层 | 项目 | 职责 | 本计划处理方式 |
|---|------|------|-------------|
| **执行引擎** | OPC-Agents | 三贤者 + 21 技能 + Web UI + CarryMem | 已在 V2 完成安装配置，本计划**直接引用** |
| **组织架构** | Hermes | 多 Profile 分工 + Wiki 共享层 | 已在 V1 完成搭建，本计划**直接引用** |
| **治理框架** | OPC Team | 任务状态机 + 决策履历 + 风险量化 + 三级记忆 + 弹性编组 | **本计划核心增量** |

---

## 一、安装 OPC Team（第 1 天）

```bash
cd C:\Users\Administrator\.claude\claude-file
git clone https://github.com/HeiGeAi/opc-team.git
cd opc-team
pip install -e .
pip install filelock   # Windows 需要
```

验证：
```bash
opc task create --title "测试任务" --ceo-input "测试安装"
opc dashboard serve
# 浏览器打开 http://127.0.0.1:8765
```

---

## 二、配置三档弹性编组（第 1 天）

### 目录划分

| 档位 | Agent 组成 | 适用场景 | 对应 Hermes Profile |
|------|-----------|---------|-------------------|
| `daily`（3 人） | CEO + COO + Strategist | 简单查询、日常任务 | Coordinator 单独处理 |
| `important`（8 人） | + Research + Product + Tech + Data + QA | 中等复杂任务 | Coordinator → Researcher + Builder |
| `full`（20 人） | CEO + 19 个 sub-agent | 战略级复杂任务 | 全员协作 |

### 配置文件

编辑 `opc-team/config.json` 的 `orchestration` 段：

```json
{
  "orchestration": {
    "main_agent_id": "ceo",
    "agent_pack": "default",
    "default_profile": "daily",
    "dispatch_profiles": {
      "daily": {
        "sub_agent_target": 3,
        "agent_ids": ["coo", "project", "strategist"]
      },
      "important": {
        "sub_agent_target": 8,
        "agent_ids": ["coo", "project", "strategist", "research", "product", "tech", "data", "qa"]
      },
      "full": {
        "sub_agent_target": 20,
        "agent_ids": "__all_sub_agents__"
      }
    }
  }
}
```

---

## 三、任务状态机（核心增量，第 2 天）

### 3.1 四级任务分级标准

| 级别 | 特征 | 编组 | 工具链 | 预计时间 |
|------|------|------|--------|---------|
| L1 | 简单查询/执行 | daily(3) | `task_flow create → assess → execution → completed` | <5 分钟 |
| L2 | 有限判断 | daily(3) | `task_flow + 1-2 部门评估` | 5-30 分钟 |
| L3 | 多方案+风险 | important(8) | `task_flow + decision_log + risk_score` | 30 分-2 小时 |
| L4 | 战略级 | full(20) | `全部 CLI 工具` | 2 小时以上 |

### 3.2 五阶段状态流转

```
created → in_strategy → in_execution → in_review → completed
                                ↗
              in_debate (L4 only)
```

**强制规则**：
- 没有定级，不进入多人协作
- 没有派发上下文，sub-agent 不允许冷启动
- L3+ 任务必须创建决策履历，否则状态机会阻止完成
- 没有风险评估和下一步动作，CEO 不允许收尾

### 3.3 命令速查

```bash
# 创建
opc task create --title "评估知识付费可行性" --ceo-input "我想做知识付费"

# 定级
opc task assess --task-id T001 --level L3 --reason "需要多方案对比+风险评估"

# 流转
opc task transition --task-id T001 --to in_strategy --actor "COO"

# 进度上报（绑定 agent-id，看板会同步）
opc task progress --task-id T001 --message "策略官开始分析" --progress 30 --agent-id strategist

# 完成
opc task transition --task-id T001 --to completed --actor "COO"

# 查看状态
opc task status --task-id T001
```

---

## 四、决策履历系统（核心增量，第 2 天）

### 适用场景
- L3/L4 任务的所有关键决策必须录入
- 需求方案选择、技术选型、定价策略、方向取舍

### 命令速查

```bash
# 创建决策（含备选方案、假设清单）
opc decision create \
  --task-id T001 \
  --decision-id D001 \
  --title "定价策略" \
  --options "方案A:低价引流(99元),方案B:高价深坑(1999元),方案C:订阅制(199元/月)" \
  --chosen "方案B" \
  --reason "高净值用户付费意愿强，口碑传播效果好" \
  --assumptions "假设1:获客成本<50元,假设2:转化率>5%,假设3:完课率>60%"

# 假设被证伪时触发重审
opc decision update-assumption \
  --decision-id D001 \
  --assumption-id 1 \
  --status "证伪" \
  --actual "实际获客成本80元" \
  --trigger-review

# 结果回填（任务完成后）
opc decision backfill \
  --decision-id D001 \
  --result "成功" \
  --metrics "转化率8%,完课率65%" \
  --lessons "高价策略需配合1v1服务"
```

### 数据存放
- 决策 JSON → `data/decisions/D001.json`
- 与 Hermes Wiki 的 `projects/{name}/decisions.md` 双向同步

---

## 五、风险量化评分（核心增量，第 2 天）

### 风险矩阵

| 等级 | 描述 | 处理方式 |
|------|------|----------|
| 1 | 可忽略 | 顺带处理 |
| 2 | 低危 | 监控即可 |
| 3 | 中危 | 必须有应对预案 |
| 4 | 高危 | 升级处理，建议暂缓 |
| 5 | 致命 | 触发停止机制 |

**计算公式**：概率(1-5) × 影响(1-5) → 风险等级(1-5)

### 命令速查

```bash
# 评估风险
opc risk assess \
  --task-id T001 \
  --risk-name "获客成本过高" \
  --probability 3 \
  --impact 4 \
  --mitigation "先做10人内测验证"

# 更新风险状态
opc risk update --risk-id R001 --status "已发生" --actual-impact 3

# 查询中高风险
opc risk list --task-id T001 --min-level 3
```

---

## 六、三级记忆系统（核心增量，第 3 天）

### 记忆层级

| 层级 | 名称 | 保留时长 | 内容 | 命令 |
|------|------|---------|------|------|
| L0 | 即时记忆 | 当前任务 | 任务过程中的原始输入输出 | `opc memory write --level L0 --task-id T001 --content "..."` |
| L1 | 短期摘要 | 7 天 | 任务完成后的压缩摘要 | `opc memory compress --task-id T001 --summary "摘要"` |
| L2 | 长期经验 | 永久 | 跨任务可复用的方法论和偏好 | `opc memory archive --category "CEO偏好" --content "..."` |

### 命令速查

```bash
# 写入即时记忆
opc memory write --level L0 --task-id T001 --content "用户要求先验证再投入"

# 任务完成后压缩到 L1
opc memory compress --task-id T001 --summary "知识付费L3任务完成，采用方案B，转化率8%"

# 沉淀长期经验到 L2
opc memory archive --category "定价经验" --content "高价策略必须配合1v1服务"

# 同步到 MEMORY.md（供所有 agent 读取）
opc memory sync --task-id T001

# 启动时读取长期记忆
opc memory read --level L2
```

### 与 OPC-Agents CarryMem 的分工

| 机制 | 存什么 | 触发方式 |
|------|--------|---------|
| **OPC Team L0/L1/L2** | 任务状态、决策摘要、假设验证结果 | 手动 `opc memory` 命令 |
| **OPC-Agents CarryMem** | 用户偏好、成功模式、失败规则 | 自动学习、自动注入策略脑 |
| **Hermes Wiki pages/** | 跨项目方法论 | 人工整理 |

---

## 七、主从编排与模型路由（第 3 天）

### CEO 主 Agent 派发模式

```bash
# CEO 向 Strategist 派发子任务
opc agent dispatch \
  --from-agent ceo \
  --to-agent strategist \
  --title "输出三套定价方案" \
  --brief "给出方案、风险和收敛建议" \
  --task-id T001 \
  --task-title "评估知识付费可行性" \
  --auto-start

# 查看待处理派发
opc agent list-assignments --open-only
```

### 独立模型路由

为不同 agent 配置不同 LLM，按精度/成本分配：

```bash
# CEO 走 Opus（复杂推理）
opc agent set-model --agent-id ceo --source custom_api \
  --provider anthropic --model claude-opus-4-8 --api-key-env ANTHROPIC_API_KEY

# Research 走 Sonnet（性价比）
opc agent set-model --agent-id research --source custom_api \
  --provider anthropic --model claude-sonnet-4-6 --api-key-env ANTHROPIC_API_KEY

# 简单任务走本地 Ollama
opc agent set-model --agent-id coo --source custom_api \
  --provider ollama --model llama3
```

---

## 八、标准化交接模板（第 4 天）

从 `opc-team/strategy/coordination/handoff-templates.md` 直接复用，核心模板：

| 模板 | 用途 | 关键字段 |
|------|------|---------|
| **标准交接单** | 主→子 agent 任务派发 | 目标、已完成、未完成、约束、验收标准 |
| **QA 通过** | 质控通过 | 证据、下一步 |
| **QA 不通过** | 质控打回 | 问题列表、预期/实际、修复建议、重试次数 |
| **升级报告** | 超时/阻塞升级给 CEO | 已尝试次数、建议决策选项 |

---

## 九、三种运行模式（第 4 天）

| 模式 | 编组 | 适用场景 | 目标时长 |
|------|------|---------|---------|
| **OPC-Micro** | daily(3) | 单个问题、单次决策 | 0.5-1 天 |
| **OPC-Sprint** | important(8) | MVP、功能迭代、业务专项 | 1-3 周 |
| **OPC-Control** | full(20) | 多团队、跨部门、持续运行 | 持续 |

启动命令：
```bash
# Micro 模式
opc task create --title "快速决策" --ceo-input "..." 
opc task assess --task-id T001 --level L1

# Sprint 模式
opc task create --title "MVP 开发" --ceo-input "..."
opc task assess --task-id T001 --level L3

# Control 模式
opc task create --title "跨部门战略" --ceo-input "..."
opc task assess --task-id T001 --level L4 --agent-profile full
```

---

## 十、可视化看板（第 4 天）

```bash
opc dashboard serve --host 127.0.0.1 --port 8765
```

看板展示：
- 当前编组档位（daily/important/full）
- 任务状态流转进度
- 各 agent 工作状态
- 风险告警
- 决策履历

---

## 十一、整体架构总览

```
用户输入
    ↓
┌───────────────────────────────────────────────────────────┐
│                    三层架构                                  │
├───────────────────────────────────────────────────────────┤
│  第一层：OPC Team 治理框架                                  │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ 任务状态机 ← 决策履历 ← 风险评分 ← 三级记忆          │   │
│  │ 弹性编组: daily(3) / important(8) / full(20)        │   │
│  │ 主从编排: CEO → COO → sub-agent                     │   │
│  └─────────────────────────────────────────────────────┘   │
│                        ↓                                    │
│  第二层：OPC-Agents 执行引擎                                │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ 🧠 策略脑 → ⚡ 执行脑 → 🔍 反思脑 → 🤝 共识引擎     │   │
│  │ 21 内置技能 + CarryMem 持久记忆 + 飞轮成长            │   │
│  │ Streamlit Web UI + 技能市场 + 知识库                  │   │
│  └─────────────────────────────────────────────────────┘   │
│                        ↓                                    │
│  第三层：Hermes 组织架构                                     │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ 四 Profile: Coordinator / Researcher / Writer / Builder│  │
│  │ 共享 Wiki: system/ + pages/ + raw/ + archive/        │   │
│  │ 交接规范 + 记忆路由 + 防污染机制                       │   │
│  └─────────────────────────────────────────────────────┘   │
│                        ↓                                    │
│  能力扩展层                                                  │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ opc-skills (10个) + OPC Team 20 角色 pack           │   │
│  │ MCP 服务 + 外接知识库 (Obsidian/Notion/飞书...)      │   │
│  └─────────────────────────────────────────────────────┘   │
└───────────────────────────────────────────────────────────┘
```

---

## 快速命令参考

```bash
# 一键安装全部
pip install opc-agents cryptography
pip install -e /path/to/opc-team
npx skills add ReScienceLab/opc-skills

# 一键启动
opc-agents        # OPC-Agents Web UI (端口 8501)
opc dashboard serve  # OPC Team 看板 (端口 8765)

# 标准任务流程
opc task create --title "..." --ceo-input "..."
opc task assess --task-id T001 --level L3
opc task transition --task-id T001 --to in_strategy --actor "COO"
opc decision create --task-id T001 ...
opc risk assess --task-id T001 ...
opc task transition --task-id T001 --to in_execution
opc task transition --task-id T001 --to completed
opc memory sync --task-id T001
```

---

## 快速启动 Checklist

- [ ] **Step 1**: 安装 OPC-Agents + 配置 .env（V2 已完成）
- [ ] **Step 2**: 安装 OPC Team + 验证 CLI
- [ ] **Step 3**: 配置三档弹性编组
- [ ] **Step 4**: 验证任务状态机（L1→L2→L3）
- [ ] **Step 5**: 配置模型路由（CEO=Opus, Research=Sonnet, COO=本地）
- [ ] **Step 6**: 跑一次完整 L3 任务流程
- [ ] **Step 7**: 跑一次 L4 战略级任务（full 满编）
- [ ] **Step 8**: 验证看板、交接模板、记忆同步
