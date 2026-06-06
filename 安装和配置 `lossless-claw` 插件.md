# 安装和配置 `lossless-claw` 插件，

能从根本上解决长对话中的“失忆”和高Token消耗问题，把AI从短期聊天工具，升级为拥有“永久记忆”的长期助手。

下面是完整的安装和配置流程。

### 🔧 安装准备

在动手前，请先确认系统满足以下要求：

- **OpenClaw**：版本需在 `2026.3.31` 以上，以支持可插拔的上下文引擎。
- **Node.js**：版本需在 `22` 以上。
- **配置好LLM提供商**：OpenClaw需要能够正常调用大语言模型，用于生成摘要。

### 🚀 安装步骤

1. **一键安装**：打开终端，运行以下命令即可完成插件安装：

   bash

   ```
   openclaw plugins install @martian-engineering/lossless-claw
   ```

   

   如果克隆代码到本地进行开发，可以执行下面命令：

   bash

   ```
   git clone https://github.com/Martian-Engineering/lossless-claw.git
   openclaw plugins install --link ./lossless-claw
   ```

   

2. **激活插件**：安装完成后，需要将其指定为默认的上下文引擎。

   - 运行以下命令来激活：

     bash

     ```
     openclaw config set plugins.slots.contextEngine lossless-claw
     ```

     

   - 或者，直接编辑配置文件 `~/.openclaw/openclaw.json`，确保包含如下内容：

     json

     ```
     {
       "plugins": {
         "slots": {
           "contextEngine": "lossless-claw"
         }
       }
     }
     ```

     

3. **重启服务**：完成安装和配置后，务必重启网关使插件生效：

   bash

   ```
   openclaw gateway restart
   ```

   

   重启后，`lossless-claw` 就已经开始工作了。

### ⚙️ 配置调优

插件支持通过环境变量或配置文件进行更精细的调优。推荐在 `~/.openclaw/openclaw.json` 中新增 `plugins.entries` 部分来配置：

json

```
{
  "plugins": {
    "slots": { "contextEngine": "lossless-claw" },
    "entries": {
      "lossless-claw": {
        "enabled": true,
        "config": {
          "freshTailCount": 32,
          "contextThreshold": 0.75,
          "incrementalMaxDepth": -1,
          "summaryProvider": "anthropic",
          "summaryModel": "claude-3-5-haiku-20241022"
        }
      }
    }
  }
}
```



各项参数的含义和推荐值如下：

| 参数                  | 含义                                                         | 推荐值                      |
| :-------------------- | :----------------------------------------------------------- | :-------------------------- |
| `freshTailCount`      | 保留最近的消息条数，作为“新鲜对话”部分，这部分内容不会被压缩。 | `32`                        |
| `contextThreshold`    | 触发压缩的上下文占用阈值（占总预算的百分比）。               | `0.75`                      |
| `incrementalMaxDepth` | DAG（有向无环图）摘要的最大深度，`-1`表示无限制。            | `-1`                        |
| `summaryProvider`     | 执行摘要压缩的模型提供商（如 `openai`, `anthropic`）。       | 留空则使用主模型            |
| `summaryModel`        | 执行摘要压缩的模型名称，建议使用更便宜、更快的模型。         | `claude-3-5-haiku-20241022` |

### 💎 核心功能：用DAG实现“永不遗忘”

`lossless-claw` 的精髓在于其“永不遗忘”的DAG机制，这使它区别于传统方案。

1. **传统方案**：对话变长后，滑动窗口会**直接丢弃**旧消息，导致信息永久丢失。
2. **Lossless-Claw方案**：
   - **持久化存储**：每条消息、每次工具调用都持久化保存在本地的 **SQLite数据库**中，永不删除。
   - **DAG摘要生成**：当对话内容增加时，会将旧消息分块压缩成摘要，并继续将摘要提炼成更高层级的节点，形成一个类似树状结构的 **DAG（有向无环图）**。
   - **动态上下文组装**：每次对话时，系统会智能地从DAG中选取相关的摘要，并把它和最新的消息拼接起来，在模型上下文窗口限制内，提供最有效的信息。
   - **精确信息召回**：利用内置工具（如 `lcm_grep`、`lcm_expand`），插件可以**精确搜索和检索**数据库中的任意原始消息，实现信息的无损回溯。

> 这个机制就像一个高效的图书馆：**SQLite是存放所有原始书籍的书库，DAG是书架上的分类索引，最终的对话则是你从书架上挑选出的几本最相关的书**，这样你既能看到全貌，又不会在书海里迷失。

### 🛠️ 必备工具：管理你的记忆库

插件提供了几个对话内命令（slash commands），用于管理和维护这个记忆库：

- `/lcm` : 查看版本、数据库状态和摘要健康度。
- `/lcm doctor` : 检查并修复损坏或不完整的摘要。
- `/lcm grep <关键词>` : 在历史记忆中快速搜索指定内容。

### 📌 日常使用须知

1. **独立与兼容**：此插件专为解决上下文问题而设计，**与之前你调整的`compaction`、`stuckSession`等配置是并行关系，不会冲突**。插件会接管原有压缩机制，实现更高效的无损管理。
2. **会话重置**：插件的DAG是按会话（session）组织的。你配置的会话重置规则（如 `session.reset.idleMinutes`）依然有效，插件会一并清理相关数据。
3. **开发注意**：如果你是开发者，插件在初始化失败时会抛出明确错误，确保及时发现问题。其核心压缩算法已通过验证，在OOLONG基准测试中将性能提升至 **74.8分**（远超Claude Code的70.3分）。

### ❓ 常见问题

- **安装失败**：请检查 `node -v` 确认Node.js版本≥22，并确保网络连接正常。
- **插件未生效**：检查 `~/.openclaw/openclaw.json` 中 `plugins.slots.contextEngine` 的值是否正确设置为 `lossless-claw`，然后重启网关。
- **SQLite 报错**：通常与权限有关。请确认当前用户对 `~/.openclaw/` 目录及其子文件有读写权限。