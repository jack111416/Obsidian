# GEO搜索优化·AI获客系统 - 详细开发计划（可执行版）

> 本计划每步均包含具体命令、文件路径、代码内容，可逐条复制执行。  
> 所有操作默认在 **Windows 10** 环境下，使用 **PowerShell** 终端。  
> 项目根目录：`C:\Users\你的用户名\openclaw\workspace-GEO`（请将 `你的用户名` 替换为实际名称）


## 准备工作：定义项目根目录变量

在 PowerShell 中执行以下命令，设置工作目录（后续所有步骤均基于此）：

```powershell
$env:OPENCLAW_WORKSPACE = "$env:USERPROFILE\openclaw\workspace-GEO"
mkdir -Force $env:OPENCLAW_WORKSPACE
cd $env:OPENCLAW_WORKSPACE
```

---


## 第一步：基础环境搭建与 OpenClaw 安装

**依赖**：无  
**预计耗时**：1 小时

### 1.1 安装 Node.js v22+

- 打开浏览器访问 https://nodejs.org/ 下载 LTS 版本（v22.x）
- 安装时勾选“自动安装必要工具”
- 安装完成后重启 PowerShell

### 1.2 安装 OpenClaw

```powershell
# 设置国内镜像加速
npm config set registry https://registry.npmmirror.com
# 全局安装 openclaw
npm install -g openclaw@latest
# 验证安装
openclaw --version
```

### 1.3 配置阿里云百炼 API

1. 登录阿里云百炼控制台 https://bailian.console.aliyun.com
2. 创建 API-Key，复制以 `sk-` 开头的密钥
3. 创建配置文件：

```powershell
# 创建目录
mkdir -Force $env:OPENCLAW_WORKSPACE\config
# 创建 API 密钥文件（请将 sk-xxxx 替换为真实密钥）
@"
DASHSCOPE_API_KEY=sk-xxxx
"@ | Out-File -FilePath $env:OPENCLAW_WORKSPACE\config\api-keys.env -Encoding utf8
```

4. 配置 OpenClaw 使用百炼模型：

```powershell
openclaw config set --section models --provider ali-bailian `
  --base-url https://dashscope.aliyuncs.com/compatible-mode/v1 `
  --api-key (Get-Content $env:OPENCLAW_WORKSPACE\config\api-keys.env | Select-String "DASHSCOPE_API_KEY=(.*)" | % { $_.Matches.Groups[1].Value })
```

### 1.4 启动 OpenClaw Gateway

```powershell
# 创建工作区
openclaw workspace init $env:OPENCLAW_WORKSPACE
# 启动 gateway（保持终端运行）
openclaw gateway start --port 18789 --verbose
```

**验证**：浏览器访问 `http://localhost:18789` 显示 OpenClaw Web 界面，且 `openclaw model test` 返回 `Connection successful`。

**完成标准**：

- [x] `openclaw --version` 正常输出版本号
- [x] Gateway 进程无报错
- [x] 模型测试通过

---


## 第二步：雷电模拟器安装与 ADB 连接

**依赖**：第一步  
**预计耗时**：1 小时

### 2.1 下载安装雷电模拟器

- 访问 https://www.ldmnq.com 下载最新版（9.0 或更高）
- 安装到默认路径 `C:\Program Files\LDPlayer\LDPlayer9`

### 2.2 创建模拟器实例

打开桌面“雷电多开器” → 点击“新建/克隆” → 创建一个实例，设置：

- 名称：`抖音号1`
- 分辨率：`1080x1920`
- CPU：2 核
- 内存：2048 MB
- 勾选“开启ROOT”和“开启ADB”

### 2.3 配置 ADB 环境变量

```powershell
# 将雷电自带的 adb 添加到 PATH
$adbPath = "C:\Program Files\LDPlayer\LDPlayer9"
[Environment]::SetEnvironmentVariable("Path", $env:Path + ";$adbPath", "User")
# 重新加载环境变量
refreshenv  # 如果无效请重启 PowerShell
```

### 2.4 连接测试

```powershell
adb devices
# 应输出：emulator-5554   device
adb shell input tap 540 1000
# 模拟器屏幕应有响应
```

**完成标准**：

- [x] 模拟器正常运行，分辨率 1080x1920
- [x] `adb devices` 显示至少一个设备
- [x] `adb shell input tap` 能点击屏幕

---


## 第三步：抖音安装与人工养号

**依赖**：第二步  
**预计耗时**：1 天（含养号等待）

### 3.1 安装抖音

- 下载抖音 APK：https://www.douyin.com/ （或通过应用商店）
- 将 APK 文件拖入模拟器窗口自动安装
- 打开抖音，使用手机号登录

### 3.2 养号操作

- 每天打开抖音刷视频 30 分钟
- 点赞、关注、评论正常内容
- 保持连续 3 天，每天发布 0 条（不要发）

**完成标准**：

- [x] 抖音 APP 可正常播放视频
- [x] 账号已登录，无异常提示
- [x] 养号满 1 天（建议 3 天）

---


## 第四步：配置 Main Agent（调度中心）

**依赖**：第一步  
**预计耗时**：2 小时

### 4.1 创建 Agent 目录结构

```powershell
mkdir -Force $env:OPENCLAW_WORKSPACE\agents\main
```

### 4.2 创建 IDENTITY.md

```powershell
@"
# IDENTITY.md - Main Agent

- **名字：** 大总管
- **角色：** 系统调度中心、项目经理
- **专注领域：** 任务拆解、流程调度、结果汇总
- **风格：** 清晰、果断、有条理

## 你不是什么
- ❌ 不是内容创作者
- ❌ 不是数据分析师

## 你是什么
- ✅ 统一交互入口
- ✅ 任务调度中心
- ✅ 流程把控者
"@ | Out-File -FilePath $env:OPENCLAW_WORKSPACE\agents\main\IDENTITY.md -Encoding utf8
```

### 4.3 创建 SOUL.md

```powershell
@"
# SOUL.md - 大总管

## 核心职责
1. **任务识别**：判断用户输入属于内容创作/数据分析/运营执行
2. **任务分发**：根据类型转发给 writer/analyst/ops agent
3. **结果汇总**：收集子Agent结果返回用户

## 任务流程卡机制（必须输出）
执行多Agent任务前输出：
```

【流程卡】
任务类型：xxx
建议链路：xxx → xxx
原因：xxx
是否需要批准：是/否

```
"@ | Out-File -FilePath $env:OPENCLAW_WORKSPACE\agents\main\SOUL.md -Encoding utf8
```

### 4.4 创建 USER.md

```powershell
@"
# USER.md - 我的用户

- **称呼：** 老板
- **时区：** Asia/Shanghai
- **项目：** GEO搜索优化抖音获客

## 偏好
- 先结论后展开
- 给出可执行方案
- 数据标注来源
"@ | Out-File -FilePath $env:OPENCLAW_WORKSPACE\agents\main\USER.md -Encoding utf8
```

### 4.5 创建 AGENTS.md（行为准则）

```powershell
@"
# AGENTS.md - 行为准则

## 委派规则
| 任务类型 | 委派Agent |
|---------|----------|
| 脚本生成/内容创作 | writer-agent |
| 数据分析/竞品监控 | analyst-agent |
| 抖音发布/互动执行 | ops-agent |

## 硬边界
- 禁止静默执行多Agent流程
- 未审批不发布内容
- 复杂任务必须走流程卡
"@ | Out-File -FilePath $env:OPENCLAW_WORKSPACE\agents\main\AGENTS.md -Encoding utf8
```

### 4.6 创建 MEMORY.md（空文件）

```powershell
New-Item -ItemType File -Path $env:OPENCLAW_WORKSPACE\agents\main\MEMORY.md -Force
```

### 4.7 注册 Main Agent 到 OpenClaw

```powershell
openclaw agents add `
  --workspace $env:OPENCLAW_WORKSPACE `
  --name main-agent `
  --alias "大总管" `
  --model bailian/qwen3.5-plus `
  main
```

### 4.8 验证 Agent

```powershell
openclaw agents list
# 应列出 main-agent
openclaw message --agent main "你好"
# 应返回“我是大总管，GEO项目的调度中心...”
```

**完成标准**：

- [x] 目录 `agents/main` 下有 5 个 .md 文件
- [x] `openclaw agents list` 显示 main-agent
- [x] 发送 `你好` 能得到符合人格设定的回复

---


## 第五步：实现脚本生成技能（Writer Agent 核心）

**依赖**：第四步  
**预计耗时**：3 小时

### 5.1 创建 Writer Agent 目录

```powershell
mkdir -Force $env:OPENCLAW_WORKSPACE\agents\writer
mkdir -Force $env:OPENCLAW_WORKSPACE\skills\geo-script-generator
```

### 5.2 创建 Writer Agent 配置文件（同第四步模板，内容聚焦脚本创作）

快速复制并修改：

```powershell
# 复制 main 配置并修改角色描述
Copy-Item "$env:OPENCLAW_WORKSPACE\agents\main\*" "$env:OPENCLAW_WORKSPACE\agents\writer\" -Recurse

# 修改 IDENTITY.md
@"
# IDENTITY.md - Writer Agent

- **名字：** 文案虾
- **角色：** 内容创作专家
- **专注领域：** 抖音脚本、文案生成
- **风格：** 网感强、懂算法、会埋钩子

## 核心能力
- 生成30秒抖音脚本（钩子+干货+CTA）
- 输出5个爆款标题
- 优化文案去除AI味
"@ | Out-File -FilePath $env:OPENCLAW_WORKSPACE\agents\writer\IDENTITY.md -Encoding utf8
```

### 5.3 创建脚本生成技能 SKILL.md

```powershell
@"
---
name: geo-script-generator
description: 根据选题生成30秒抖音脚本
triggers: ["生成脚本", "写脚本", "创作脚本"]
---

# 脚本生成技能

## 参数
| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| topic | string | 是 | 选题关键词或一句话描述 |
| style | string | 否 | 风格：焦虑型/干货型/案例型，默认干货型 |

## 执行流程

1. 调用大模型生成脚本，结构如下：
   - 0-3秒：痛点钩子（占1句话）
   - 4-20秒：3步解决方案（每步1-2句话）
   - 21-30秒：CTA行动号召（私信/评论引导）

2. 同时生成3个备选标题

3. 返回 JSON 格式：
   {
     "script": "完整脚本文本",
     "titles": ["标题1", "标题2", "标题3"]
   }

## 示例输入
topic: "GEO搜索优化"
style: "焦虑型"

## 示例输出
{
  "script": "你的企业在AI搜索里找不到？63%的用户用豆包代替百度...",
  "titles": ["3步让AI主动推荐你的企业", "你的企业正在被AI淘汰？"]
}
"@ | Out-File -FilePath $env:OPENCLAW_WORKSPACE\skills\geo-script-generator\SKILL.md -Encoding utf8
```

### 5.4 注册 Writer Agent 并安装技能

```powershell
openclaw agents add `
  --workspace $env:OPENCLAW_WORKSPACE `
  --name writer-agent `
  --alias "文案虾" `
  --model bailian/qwen3.5-plus `
  writer

openclaw skills install $env:OPENCLAW_WORKSPACE\skills\geo-script-generator
```

### 5.5 测试脚本生成

```powershell
openclaw message --agent writer "生成关于GEO搜索优化的抖音脚本"
```

**完成标准**：

- [x] Writer Agent 能返回包含“钩子+干货+CTA”结构的脚本
- [x] 输出包含 3 个标题备选
- [x] 脚本长度 ≤ 200 字

---


## 第六步：实现 TTS 配音 + 简单视频合成

**依赖**：第五步  
**预计耗时**：4 小时

### 6.1 安装 FFmpeg

```powershell
# 使用 winget 安装
winget install FFmpeg
# 或手动下载 https://ffmpeg.org/download.html 并添加到 PATH
```

### 6.2 创建视频合成技能

```powershell
mkdir -Force $env:OPENCLAW_WORKSPACE\skills\video-composer
```

创建 `SKILL.md`：

```powershell
@"
---
name: video-composer
description: 将脚本转为带配音和字幕的短视频
triggers: ["合成视频", "生成视频"]
---

# 视频合成技能

## 参数
| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| script | string | 是 | 脚本文本 |
| output_path | string | 否 | 输出路径，默认 output/videos/视频名.mp4 |

## 执行步骤

1. **生成配音**：使用 `@openclaw/tts-edge` 技能
   - 语速 1.1，声音 zh-CN-XiaoxiaoNeural
   - 输出 wav 文件到临时目录

2. **生成背景图**：调用大模型生成画面描述，然后用 `@openclaw/image-gen` 生成 3 张 9:16 图片

3. **合成视频**：使用 ffmpeg 将图片轮播 + 配音合成
   ```bash
   ffmpeg -y -loop 1 -i image1.jpg -i audio.wav -c:v libx264 -c:a aac -pix_fmt yuv420p -shortest -vf "fps=1,scale=1080:1920" output.mp4
```

4. **添加字幕**：使用 `@openclaw/auto-caption` 技能（需额外安装）

5. 返回最终视频路径

**注意**：如果`auto-caption`不可用，跳过字幕，后续手动添加。
"@ | Out-File -FilePath $env:OPENCLAW_WORKSPACE\skills\video-composer\SKILL.md -Encoding utf8

```
### 6.3 安装所需 OpenClaw 技能
```powershell
openclaw skills install @openclaw/tts-edge
openclaw skills install @openclaw/image-gen
openclaw skills install @openclaw/auto-caption  # 可选
```

### 6.4 测试合成

```powershell
# 先让 writer-agent 生成脚本，保存为变量
$script = openclaw message --agent writer "生成一条关于GEO优化的脚本" | ConvertFrom-Json
# 调用合成技能（需在 OpenClaw 会话中执行，此处仅为示例）
```

**完成标准**：

- [x] 能生成一个 10-15 秒的 MP4 文件
- [x] 视频有配音和静态背景图
- [x] 视频分辨率 1080x1920

---


## 第七步：单账号手动发布视频（半自动 ADB 脚本）

**依赖**：第三步、第六步  
**预计耗时**：3 小时

### 7.1 编写 ADB 发布脚本

创建文件 `$env:OPENCLAW_WORKSPACE\scripts\publish_video.ps1`：

```powershell
param(
    [string]$videoPath,
    [string]$title,
    [string]$deviceId = "emulator-5554"
)

# 1. 推送视频到模拟器
adb -s $deviceId push $videoPath "/sdcard/DCIM/Camera/video_temp.mp4"

# 2. 打开抖音
adb -s $deviceId shell am start -n com.ss.android.ugc.aweme/.main.MainActivity
Start-Sleep -Seconds 3

# 3. 点击底部“+”按钮（坐标根据分辨率调整，1080x1920下约540,1800）
adb -s $deviceId shell input tap 540 1800
Start-Sleep -Seconds 2

# 4. 点击“相册”或“选择文件”（位置取决于抖音版本，需实际测试）
adb -s $deviceId shell input tap 200 500
Start-Sleep -Seconds 2

# 5. 选择第一个视频
adb -s $deviceId shell input tap 540 1000
Start-Sleep -Seconds 2

# 6. 点击“下一步”
adb -s $deviceId shell input tap 980 1900
Start-Sleep -Seconds 2

# 7. 输入标题（需处理中文）
adb -s $deviceId shell input text $title
Start-Sleep -Seconds 1

# 8. 点击“发布”
adb -s $deviceId shell input tap 540 2100

Write-Host "发布指令已发送，请检查模拟器画面"
```

### 7.2 执行测试

```powershell
# 假设已有一个视频文件 C:\video.mp4
.\scripts\publish_video.ps1 -videoPath "C:\video.mp4" -title "GEO优化3步走"
```

**完成标准**：

- [x] 视频成功出现在抖音个人主页
- [x] 标题正确显示

---


## 第八步：创建 Ops Agent，实现自动发布技能

**依赖**：第七步  
**预计耗时**：4 小时

### 8.1 创建 Ops Agent 目录

```powershell
mkdir -Force $env:OPENCLAW_WORKSPACE\agents\ops
mkdir -Force $env:OPENCLAW_WORKSPACE\skills\douyin-publish
```

### 8.2 创建 Ops Agent 人格配置（参考 main，角色改为执行者）

创建 `IDENTITY.md` 等文件，内容聚焦“执行发布、私信回复、设备控制”。

### 8.3 创建发布技能 SKILL.md

```powershell
@"
---
name: douyin-publish
description: 通过ADB在指定模拟器发布抖音视频
triggers: ["发布视频", "发抖音"]
---

# 抖音发布技能

## 参数
| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| device_id | string | 是 | ADB设备ID，如 emulator-5554 |
| video_path | string | 是 | 本地视频文件路径 |
| title | string | 是 | 视频标题（≤55字） |

## 执行步骤

1. 使用 `shell_exec` 调用 `adb -s {device_id} push {video_path} /sdcard/DCIM/Camera/temp.mp4`
2. 继续依次执行 ADB 命令（同第七步的脚本）
3. 返回 `{"status": "success", "message": "视频已发布"}`

## 错误处理
- 若 adb 命令失败，重试最多 3 次
- 若视频推送失败，返回错误信息
"@ | Out-File -FilePath $env:OPENCLAW_WORKSPACE\skills\douyin-publish\SKILL.md -Encoding utf8
```

### 8.4 注册 Ops Agent 并安装技能

```powershell
openclaw agents add `
  --workspace $env:OPENCLAW_WORKSPACE `
  --name ops-agent `
  --alias "运营虾" `
  --model bailian/qwen3.5-plus `
  ops

openclaw skills install $env:OPENCLAW_WORKSPACE\skills\douyin-publish
```

### 8.5 测试

```powershell
openclaw message --agent ops "在设备 emulator-5554 上发布视频 D:\output\test.mp4，标题是‘测试发布’"
```

**完成标准**：

- [x] Ops Agent 能解析指令并调用 ADB 脚本
- [x] 视频成功发布到抖音

---


## 第九步：多账号管理与独立 IP 配置

**依赖**：第八步  
**预计耗时**：5 小时

### 9.1 创建第二个模拟器实例

打开雷电多开器，克隆“抖音号1”为“抖音号2”，启动后 ADB 设备 ID 应为 `emulator-5556`

### 9.2 配置独立代理（使用 ProxyCap）

1. 购买两个静态代理 IP（例如 109.xx.xx.xx:port）
2. 下载安装 ProxyCap
3. 为每个 `Ld9BoxHeadless.exe` 进程绑定不同代理：
   - 规则1：进程路径 `C:\Program Files\LDPlayer\LDPlayer9\Ld9BoxHeadless.exe` 参数包含 `--index 0` → 代理IP1
   - 规则2：参数包含 `--index 1` → 代理IP2
4. 验证：在两个模拟器中访问 `ip138.com` 显示不同 IP

### 9.3 更新 Main Agent 路由规则

编辑 `$env:OPENCLAW_WORKSPACE\agents\main\AGENTS.md`，增加设备映射表：

```markdown
## 设备映射
| 抖音号 | 模拟器 | ADB ID | 代理IP |
|-------|--------|--------|--------|
| 号1   | 实例1  | emulator-5554 | 109.xx.xx.1 |
| 号2   | 实例2  | emulator-5556 | 109.xx.xx.2 |
```

### 9.4 修改发布技能，支持多设备

在 `skills\douyin-publish\SKILL.md` 中增加 `device_id` 参数传递。

**完成标准**：

- [x] 两个模拟器可同时在线，IP 不同
- [x] Ops Agent 能通过 `device_id` 区分发布目标

---


## 第十步：批量群发工作流

**依赖**：第九步  
**预计耗时**：3 小时

### 10.1 创建工作流定义文件

```yaml
# $env:OPENCLAW_WORKSPACE\workflows\batch-publish.yaml
name: batch-publish
description: 批量发布视频到多个抖音号
steps:
  - prompt: "请提供视频文件路径和要发布的设备ID列表（用逗号分隔）"
  - set: video_path = {{input.video_path}}
  - set: device_ids = {{input.device_ids | split(',')}}
  - for each device_id in device_ids:
    - call_skill: douyin-publish
      params:
        device_id: {{device_id}}
        video_path: {{video_path}}
        title: {{input.title}}
    - wait: 300  # 间隔5分钟
  - respond: "批量发布完成"
```

### 10.2 注册工作流

```powershell
openclaw workflow add --file $env:OPENCLAW_WORKSPACE\workflows\batch-publish.yaml
```

### 10.3 测试

```powershell
openclaw workflow run batch-publish --params '{"video_path":"D:\\output\\geo.mp4","device_ids":"emulator-5554,emulator-5556","title":"GEO优化"}'
```

**完成标准**：

- [x] 能按顺序发布到两个设备
- [x] 间隔生效
- [x] 生成发布报告

---


## 第十一步：私信自动回复与线索池（基础版）

**依赖**：第八步  
**预计耗时**：6 小时

### 11.1 创建回复规则文件

```powershell
@"
# $env:OPENCLAW_WORKSPACE\config\reply_rules.json
{
  "rules": [
    {"keywords": ["多少钱", "价格", "收费"], "reply": "您好，GEO优化服务根据企业规模和需求定制，通常起步价在XXX。方便告知您的行业吗？我发您详细方案。"},
    {"keywords": ["案例", "效果", "有案例吗"], "reply": "有的，我们服务了XX家制造企业，平均询盘提升300%。私信我您的行业，发您专属案例。"},
    {"keywords": ["怎么合作", "合作流程"], "reply": "合作流程：1. 初步诊断 2. 方案定制 3. 执行优化 4. 数据复盘。请问您企业目前有做搜索优化吗？"}
  ],
  "default_reply": "感谢您的关注！关于GEO搜索优化，我可以为您提供免费诊断。请私信发送“诊断+您的行业”，我会尽快回复。"
}
"@ | Out-File -FilePath $env:OPENCLAW_WORKSPACE\config\reply_rules.json -Encoding utf8
```

### 11.2 创建私信监控技能

```powershell
mkdir -Force $env:OPENCLAW_WORKSPACE\skills\private-reply
```

`SKILL.md`:

```markdown
---
name: private-reply
description: 监控抖音私信并自动回复
---

# 私信回复技能

## 执行流程
1. 通过 adb shell 获取抖音通知栏内容（使用 `dumpsys notification`）
2. 解析出新私信的用户ID和消息内容
3. 匹配 reply_rules.json 中的关键词
4. 使用 adb input text 发送回复
5. 将对话记录追加到 `data/leads.json`

## 注意
- 回复间隔随机15-47秒
- 单日回复上限30条
```

### 11.3 运行监控（定时任务）

```powershell
# 创建 PowerShell 后台任务
Start-Job -Name "PrivateReply" -ScriptBlock {
    while($true) {
        openclaw skill run private-reply --device emulator-5554
        Start-Sleep -Seconds 120
    }
}
```

**完成标准**：

- [x] 收到含“多少钱”的私信，自动回复话术
- [x] 线索池文件 `leads.json` 有记录

---


## 第十二步：集成阿里云百炼 Wan2.7 视频生成

**依赖**：第六步  
**预计耗时**：4 小时

### 12.1 获取百炼视频生成 API 密钥

已在第一步配置过 DashScope API-Key，它同时支持视频生成。

### 12.2 创建高级视频合成技能

```powershell
mkdir -Force $env:OPENCLAW_WORKSPACE\skills\ai-video-gen
```

`SKILL.md` 核心内容：

```markdown
---
name: ai-video-gen
description: 使用阿里百炼Wan2.7生成动态视频
---

# AI视频生成技能

## 参数
- prompt: 画面描述
- duration: 5-15秒，默认10
- resolution: 720P 或 1080P

## 调用方法
使用 `http_request` 调用 DashScope API：
```http
POST https://dashscope.aliyuncs.com/api/v1/services/aigc/video-generation/video-synthesis
Authorization: Bearer {{env.DASHSCOPE_API_KEY}}
Content-Type: application/json
X-DashScope-Async: enable

{
  "model": "wan2.7-t2v",
  "input": { "prompt": "{{prompt}}" },
  "parameters": { "duration": {{duration}}, "resolution": "{{resolution}}" }
}
```

轮询任务状态，下载视频到 `output/videos/`。

```
### 12.3 测试生成
```powershell
openclaw skill run ai-video-gen --params '{"prompt":"商务科技感，数据流动，蓝色光效，9:16竖屏","duration":10}'
```

**完成标准**：

- [x] 能生成 10 秒 1080P 视频
- [x] Writer Agent 默认使用此技能代替静态图轮播

---


## 第十三步：开发 Web 管理界面（仪表盘 + 内容工厂）

**依赖**：第十二步（可先不依赖此步，但建议顺序）  
**预计耗时**：2 天

### 13.1 创建前端目录

```powershell
mkdir -Force $env:OPENCLAW_WORKSPACE\web
cd $env:OPENCLAW_WORKSPACE\web
```

### 13.2 创建 index.html（简易仪表盘）

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>GEO获客系统</title>
    <style>
        body { font-family: Arial; margin: 20px; }
        .card { border: 1px solid #ccc; padding: 15px; margin: 10px; display: inline-block; width: 200px; }
    </style>
</head>
<body>
    <h1>GEO搜索优化·AI获客系统</h1>
    <div class="card">总视频数: <span id="totalVideos">0</span></div>
    <div class="card">总播放量: <span id="totalPlays">0</span></div>
    <div class="card">私信数: <span id="totalMsgs">0</span></div>
    <div class="card">线索数: <span id="totalLeads">0</span></div>
    <script>
        // 调用 OpenClaw REST API（需先开启 API 访问）
        fetch('http://localhost:18789/api/stats')
            .then(res => res.json())
            .then(data => {
                document.getElementById('totalVideos').innerText = data.videos;
                document.getElementById('totalPlays').innerText = data.plays;
                document.getElementById('totalMsgs').innerText = data.messages;
                document.getElementById('totalLeads').innerText = data.leads;
            });
    </script>
</body>
</html>
```

### 13.3 启动简易 Web 服务

```powershell
# 使用 Python 快速启动
python -m http.server 3000 --directory $env:OPENCLAW_WORKSPACE\web
```

**完成标准**：

- [x] 浏览器打开 `http://localhost:3000` 看到界面
- [x] 数据能通过 API 获取（需实现 `/api/stats`，可用 OpenClaw Gateway 插件）

（完整页面开发工作量较大，建议后续迭代）

---


## 第十四步：剩余页面与完整功能闭环（略）

由于时间关系，此步为占位，实际开发时逐个完成账号中心、发布任务、互动管理等页面。

**完成标准**：所有页面功能符合项目说明文档。

---


## 第十五步：整体集成测试与风控优化

**依赖**：第十四步  
**预计耗时**：2 天

### 15.1 运行端到端测试

```powershell
# 测试全流程
openclaw message --agent main "生成一条关于GEO优化的脚本并合成为视频，然后发布到两个抖音号，间隔3分钟"
```

### 15.2 配置风控参数

编辑 `$env:OPENCLAW_WORKSPACE\config\anti_fraud.yaml`：

```yaml
publish_interval_seconds: 300   # 同一账号间隔5分钟
max_daily_publish_per_account: 5
reply_delay_min: 15
reply_delay_max: 47
```

### 15.3 运行健康检查

```powershell
openclaw doctor --full
```

**完成标准**：

- [x] 连续运行 3 天无封号
- [x] 所有 API 调用日志完整
- [x] 生成测试报告


## 技术栈总览（补充）

| 类别       | 技术                      | 版本/备注      |
| ---------- | ------------------------- | -------------- |
| 操作系统   | Windows 10/11             | 开发及运行环境 |
| 运行环境   | Node.js 22+               | 必需           |
| Agent 框架 | OpenClaw                  | 最新版         |
| 大模型     | 阿里百炼 Qwen3.5-Plus     | API            |
| 视频生成   | 阿里百炼 Wan2.7-t2v       | API            |
| TTS        | Edge TTS (OpenClaw skill) | 免费           |
| 安卓控制   | ADB                       | 雷电模拟器自带 |
| 模拟器     | 雷电模拟器 9.0            | 多开           |
| 代理工具   | ProxyCap                  | 付费/试用      |
| 前端       | HTML/CSS/JS               | 原生           |
| 进程管理   | PM2                       | 守护 Gateway   |
| 版本控制   | Git                       | 可选           |

---

**每步完成标准已明确，建议每完成一步提交 Git 记录，便于回溯。**