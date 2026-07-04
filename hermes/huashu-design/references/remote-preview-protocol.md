# Remote Preview Protocol · Termux / Headless 环境

在无桌面浏览器环境（Termux / SSH / CI）交付 HTML 设计时的预览与验证规范。

## 🖥️ 本地 HTTP 服务器

### 启动

```bash
# 项目目录下启动
python3 -m http.server 8899 --bind 0.0.0.0

# 后台运行
python3 -m http.server 8899 --bind 0.0.0.0 > /dev/null 2>&1 &
```

### 重启流程（Termux 实测）

```bash
pkill -f "http.server 8899"
sleep 1
python3 -m http.server 8899 --bind 0.0.0.0
```

### WiFi IP 发现

```bash
ip addr show wlan0 | grep inet | head -1
# 输出示例: inet 192.168.31.68/24 ...
```

### 健康检查

```bash
curl -s -o /dev/null -w "%{http_code}" http://localhost:8899/
# 期望输出: 200
```

## 🌐 多文件 deck 的预览路径

多文件架构（`index.html` + `slides/*.html`）取 **index.html 的 URL** 作为预览入口：
- `http://192.168.31.68:8899/` → 自动打开 3D 概览墙
- 点卡片或「▶ 开始演示」进入全屏翻页
- 概览模式：`?ov=grid|gallery` 强制指定

## 📸 无 GUI 时的截图方案

Termux 没有 GUI 浏览器，但可用 headless Chromium：

```bash
# 截图（1920×1080）
chromium-browser --headless --disable-gpu \
  --screenshot=out.png \
  --window-size=1920,1080 \
  file:///path/to/slide.html
```

**限制**：
- Chromium 不带彩色 emoji 字体，emoji 会显示为方框
- Google Fonts 走代理 TLS 会炸，须 self-host 或用系统字体

## 📂 文件编辑纪律

### 🛑 常见坑：编辑了错误文件

多文件架构下很容易出现：开着旧文件 `ppts/report-showcase.html` 修改，但 HTTP 服务器 serve 的是 `ppts/index.html`。改了半天看不到变化。

**铁律**：始终确认 HTTP 服务目录和编辑文件是同一目录。修改后先看 `deck_index.html` 的 MANIFEST 里写的 `file:` 路径，对着那个路径编辑。

### 编辑策略

| 策略 | 适用 | 说明 |
|------|------|------|
| 直接 edit `slides/XX.html` | 正式 | 多文件架构，每页独立 |
| 先 `cp` 备份再改 | 重大变动 | 回滚保险 |

## 🔧 进程管理速查

| 目标 | 方法 |
|------|------|
| 启动长期服务 (HTTP) | `terminal(background=true, notify_on_complete=false)` |
| 启动有明确结束的任务 | `terminal(background=true, notify_on_complete=true)` |
| 查看后台进程状态 | `process(action='poll', session_id=...)` |
| 终止进程 | `process(action='kill', session_id=...)` |

## 📤 Feishu 媒体发送

Termux 里 Feishu 发图用 `MEDIA:/absolute/path` 协议，不经过 base64 编码：

```
MEDIA:/data/data/com.termux/files/home/.hermes/skills/.../file.png
```

Chat ID 格式：`feishu:oc_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`

发送后 verify：`send_message` 返回 `success: true` 且有 `message_id`。

## ⚠️ Termux 特有约束

| 约束 | 说明 | 绕法 |
|------|------|------|
| 无 `npx` / Playwright | npm 全局工具大多未装 | headless chromium + python http.server |
| Google Fonts TLS 握手失败 | Termux 代理 + 证书配置不完整 | 自托管字体到 `shared/fonts/` 或只用系统字体 |
| 无 GUI 浏览器 | 无法实时预览 | 依赖 headless 截图 + HTTP server + 用户手机/电脑同 WiFi |
| 进程随 Termux 杀 | App 杀后台 = HTTP 服务也死 | 用户注意不要清后台；文件已持久化到磁盘 |
