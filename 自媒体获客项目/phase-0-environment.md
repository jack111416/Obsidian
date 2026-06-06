# Phase 0: 环境准备与基础设施

> ⏳ 状态：待开始
> 预计耗时：1-2小时
> 依赖：无（首阶段）
> 后续阶段：Phase 1 (账号登录)

---

## 🎯 阶段目标

配置OpenClaw运行环境，配对Android设备，验证核心技能可用性，创建项目骨架。

**成功标志**:
- ✅ OpenClaw Gateway正常运行
- ✅ Android设备通过Nodes配对成功
- ✅ browser技能可访问抖音网页版
- ✅ 项目目录结构和配置文件就绪

---

## 📋 任务清单（按执行顺序）

### Task 0.1: 检查OpenClaw版本并启用必要插件

**目标**: 确保OpenClaw版本≥2026.5.27，启用browser、nodes、feishu相关插件

**涉及文件**:
- 无（系统命令操作）

**具体步骤**:
1. 运行 `openclaw gateway status` 查看服务状态
2. 运行 `openclaw plugins list` 查看插件列表
3. 检查以下插件是否为 **enabled** 状态：
   - `@openclaw/browser-plugin` (浏览器自动化)
   - `Device Pairing` (设备配对)
   - `@larksuite/openclaw-lark` (飞书集成)
4. 如有禁用，需要在 `~/.openclaw/openclaw.json` 中启用，然后重启gateway

**完成标准**:
```bash
# 执行后应看到:
openclaw gateway status  # 显示 "status: running"
openclaw plugins list | findstr browser  # 显示 enabled
openclaw plugins list | findstr "Device Pairing"  # 显示 enabled
```

**验证命令**:
```bash
openclaw gateway status
openclaw plugins list
```

**常见问题**:
- 插件未启用 → 编辑 `openclaw.json`: `plugins.entries.<pluginId>.enabled = true`
- Gateway未运行 → `openclaw gateway run` 或 `openclaw gateway start`

---

### Task 0.2: 创建项目目录结构

**目标**: 在 `workspace-GEO` 下创建标准项目结构

**涉及文件**:
- 创建以下目录和文件

**具体步骤**:
1. 进入工作区: `cd C:\Users\Administrator\.openclaw\workspace-GEO`
2. 创建目录树：
```bash
mkdir PHASES SKILLS CONFIG DATA DB docs
mkdir SKILLS\{douyin-upload,video-synthesis,account-manager}
mkdir DATA\{accounts,videos,scripts,topics,logs}
mkdir DB\{bitable}
```

3. 创建配置文件模板：
   - `CONFIG/openclaw.json` - 从主配置复制并添加douyin相关设置
   - `CONFIG/skills.json` - 自定义技能注册表
   - `CONFIG/auth-profiles.json` - 账号认证配置模板

4. 创建占位文件：
   - `PHASES/.gitkeep`
   - `SKILLS/.gitkeep`
   - `CONFIG/.gitkeep`
   - `DATA/.gitkeep`
   - `DB/.gitkeep`
   - `docs/.gitkeep`

**完成标准**:
```powershell
# 验证目录结构
Get-ChildItem -Recurse -Force | Select-Object FullName
# 应看到所有预期目录
```

**验证命令**:
```powershell
dir C:\Users\Administrator\.openclaw\workspace-GEO\PHASES
dir C:\Users\Administrator\.openclaw\workspace-GEO\SKILLS
```

---

### Task 0.3: 初始化配置模板

**目标**: 创建可用的配置模板，包含技能注册和账号配置框架

**涉及文件**:
- `CONFIG/openclaw.json` (补充douyin相关配置)
- `CONFIG/skills.json` (自定义技能清单)
- `CONFIG/auth-profiles.json` (账号认证模板)

**具体步骤**:

**1. 扩展 `openclaw.json`** (如果已有则修改):
```json5
{
  // ... 现有配置 ...
  plugins: {
    entries: {
      // 已有插件确保启用
      "browser": { enabled: true },
      "device-pair": { enabled: true },
      "openclaw-lark": { enabled: true },
      // 新增可选
      "canvas": { enabled: false }  // 后续可能需要
    }
  },
  skills: {
    load: {
      extraDirs: ["SKILLS"]  // 指向自定义技能目录
    }
  },
  agents: {
    defaults: {
      skills: ["taskflow", "browser", "nodes", "feishu-bitble"]
    }
  }
}
```

**2. 创建 `CONFIG/skills.json`** (自定义技能注册):
```json5
{
  "customSkills": [
    {
      "id": "douyin-upload",
      "name": "抖音上传",
      "path": "SKILLS/douyin-upload",
      "enabled": true
    },
    {
      "id": "video-synthesis",
      "name": "视频合成",
      "path": "SKILLS/video-synthesis",
      "enabled": true
    },
    {
      "id": "account-manager",
      "name": "账号管理",
      "path": "SKILLS/account-manager",
      "enabled": true
    }
  ]
}
```

**3. 创建 `CONFIG/auth-profiles.json`** (模板):
```json5
{
  "douyinAccounts": [
    {
      "id": "douyin-test-01",
      "name": "测试账号1",
      "platform": "douyin",
      "sessionFile": "DATA/accounts/douyin-test-01.session.json",
      "nodeId": null,  // 绑定设备后填写
      "status": "inactive",
      "createdAt": "2026-05-31T15:40:00+08:00"
    }
  ],
  "apiKeys": {
    "alibaba": null,  // 填写阿里百炼API Key
    "tencentTts": null,  // 腾讯云TTS密钥
    "stepfun": null  // StepFun API Key
  }
}
```

**完成标准**:
- 三个配置文件存在且格式正确（可通过`openclaw gateway config.get`验证）
- `skills.json` 中的路径实际存在
- `auth-profiles.json` 包含至少一个账号模板

**验证命令**:
```bash
# 检查配置文件语法
cat CONFIG/openclaw.json | jq .  # 或任何JSON验证工具
cat CONFIG/skills.json | jq .
cat CONFIG/auth-profiles.json | jq .
```

---

### Task 0.4: Android设备配对

**目标**: 将Android设备通过Nodes系统配对到OpenClaw

**涉及文件**:
- 无（节点配对是运行时操作）

**具体步骤**:

1. **准备Android设备**:
   - 安装OpenClaw安卓应用 (从应用商店或APK)
   - 确保设备和电脑在同一网络
   - 开启USB调试（如需有线连接）

2. **生成配对码**:
   在电脑上运行:
   ```bash
   openclaw nodes pending
   ```
   会显示待配对的设备二维码和配对码

3. **设备端操作**:
   - 打开OpenClaw App
   - 点击"配对设备"
   - 扫描二维码或输入配对码
   - 授权设备权限（相机、通知等）

4. **确认配对成功**:
   ```bash
   openclaw nodes status
   ```
   应看到设备列在 `paired` 列表中，状态为 `online`

5. **记下设备ID**:
   从 `nodes status` 输出中找到设备的 `id`（如 `android-123456`）
   更新 `CONFIG/auth-profiles.json`:
   ```json5
   {
     "douyinAccounts": [
       {
         "id": "douyin-test-01",
         "nodeId": "android-123456"  // ← 填写这里
       }
     ]
   }
   ```

**完成标准**:
```bash
# 执行后应看到:
openclaw nodes status
# 输出包含:
# [
#   {
#     "id": "android-xxxxx",
#     "name": "Android Device",
#     "status": "online",
#     "platform": "android"
#   }
# ]
```

**验证命令**:
```bash
openclaw nodes status
# 设备数量 >= 1 且状态为 online
```

**常见问题**:
- 设备不在同一网络 → 检查防火墙，确保18789端口开放
- 配对码过期 → 重新运行 `nodes pending` 刷新
- 设备离线 → 检查App是否在后台运行，电量优化设置

---

### Task 0.5: 验证Nodes基础功能

**目标**: 测试通过Nodes控制设备的能力

**涉及文件**:
- 无（测试命令）

**具体步骤**:

**1. 测试相机快照**:
```bash
openclaw nodes action=camera_snap node=<你的device-id> facing=back
```
应返回图片保存路径，如 `{"saved_path": "C:\\...\\snapshot.jpg"}`

**2. 测试屏幕截图**:
```bash
openclaw nodes action=screen_record node=<your-device-id> duration=3
```
录制3秒屏幕后返回视频文件路径

**3. 测试通知监听**:
```bash
openclaw nodes action=notifications_list node=<your-device-id> limit=10
```
查看最近的设备通知（用于后续私信监听）

**完成标准**:
- `camera_snap` 成功返回图片路径，文件存在
- `screen_record` 成功返回视频路径，文件存在
- `notifications_list` 返回JSON数组（可能为空，但命令不报错）

**验证命令**:
```bash
# 检查文件是否存在
dir <返回的saved_path>
dir <返回的视频路径>
```

---

### Task 0.6: 验证Browser自动化功能

**目标**: 确认browser技能可正常访问抖音网页版

**涉及文件**:
- 无（测试命令）

**具体步骤**:

**1. 启动browser会话**:
```bash
# OpenClaw中调用browser工具
action: start
```

**2. 访问抖音网页版**:
```bash
action: open
url: https://www.douyin.com
```

**3. 等待页面加载**:
```bash
action: wait
loadState: networkidle
```

**4. 截图验证**:
```bash
action: screenshot
type: png
```

**5. 检查页面元素**:
```bash
action: snapshot
refs: aria
```
查看返回的DOM结构，确认有登录按钮或内容区域

**完成标准**:
- Browser成功启动并访问抖音
- Screenshot返回图片文件
- Snapshot包含抖音页面元素（如 `.login-btn` 或视频列表容器）

**验证命令**:
```bash
# 在OpenClaw会话中，使用browser工具链式调用
# 截图文件应存在
```

**注意事项**:
- 抖音可能有反爬机制，首次访问可能显示验证码
- 如果遇到验证，手动在打开的浏览器窗口中完成验证
- 建议使用 `profile="user"` 使用已登录的Chrome会话（更稳定）

---

### Task 0.7: 创建开发日志初始化脚本

**目标**: 自动化创建DEV_LOG.md和阶段文档的辅助脚本

**涉及文件**:
- `scripts/init-log.js` (可选)
- `DEV_LOG.md` (已创建)
- `PHASES/` (已创建)

**具体步骤**:
1. 创建 `scripts/` 目录
2. 可选：编写Node.js脚本自动生成阶段文档模板
3. 本阶段不强制要求自动化，手动创建已足够

**完成标准**:
- `DEV_LOG.md` 存在且格式正确
- `PHASES/` 目录非空
- 每个Phase文档有标准模板（目标、任务、验证方法）

---

## ✅ Phase 0 完成检查清单

在进入Phase 1之前，必须全部满足：

- [ ] OpenClaw Gateway正常运行 (`openclaw gateway status` → running)
- [ ] browser插件已启用 (`openclaw plugins list` → enabled)
- [ ] nodes/device-pair插件已启用
- [ ] 项目目录结构完整 (`PHASES/`, `SKILLS/`, `CONFIG/`, `DATA/`, `DB/`存在)
- [ ] 三个配置文件 (`openclaw.json`, `skills.json`, `auth-profiles.json`) 已创建且JSON格式正确
- [ ] Android设备已配对成功 (`openclaw nodes status` → online设备≥1)
- [ ] 获取了设备ID并已记录到 `CONFIG/auth-profiles.json`
- [ ] 成功执行 `nodes action=camera_snap` 并获取图片
- [ ] 成功执行 `browser open https://www.douyin.com` 并截图
- [ ] 更新了 `DEV_LOG.md`，标记Phase 0为进行中，记录设备ID和配置信息

---

## 🐛 已知问题与解决方案

| 问题 | 原因 | 解决方案 |
|------|------|----------|
| browser启动失败 | Chrome未安装或路径不对 | 安装Chrome或设置 `browser.executablePath` |
| nodes找不到设备 | 设备未安装App或网络不通 | 检查App版本、网络、防火墙 |
| 插件不启用 | 配置中 `enabled: false` | 编辑 `openclaw.json` 设为 `true` 并重启gateway |
| 配置文件JSON错误 | 尾逗窗前不允许注释 | 使用JSON5格式或确保严格JSON |

---

## 📞 需要人工介入的点

1. **Android设备安装App**: 需要人工在手机上操作
2. **抖音网页版验证码**: 首次访问可能需手动验证
3. **获取抖音Cookie**: Phase 1需要人工登录一次

---

## 🔄 下一步

完成Phase 0全部checklist后，开始 **Phase 1: 账号登录与会话持久化**

---

> **写给新AI的话**:
>
> 如果你是新的会话，请先读取:
> 1. `DEV_LOG.md` - 了解项目整体状态
> 2. 本文件 - 查看当前阶段任务
>
> 当前阶段是Phase 0，目标是环境准备。所有任务都是独立的一次性配置。
> 按顺序执行Task 0.1到0.6，每完成一个更新 `DEV_LOG.md` 的进度表。
> 遇到问题先尝试解决，无法解决则记录到DEV_LOG的"开发日志"部分并暂停。
>
> 这个阶段完成后，用户将提供Android设备和手动登录抖音网页版，为Phase 1做准备。
