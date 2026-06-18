在 Termux 上安装 OpenClaw 主要有两种方式：**一是直接安装专用的 Flutter APK 应用（最省心）**，**二是在 Termux 命令行中通过 npm 安装（更极客）**。

### 📱 方案一：安装 Flutter APK 应用（最简单，推荐）

这种方式提供了一个图形界面，操作起来最方便，几乎不需要敲命令[](https://libraries.io/npm/openclaw-termux)。

1. **下载 APK**：从项目的 GitHub 发布页下载最新的 APK 文件[](https://www.160.com/article/12654.html)。
    
    - 下载地址：[https://github.com/mithun50/openclaw-termux/releases](https://github.com/mithun50/openclaw-termux/releases)[](https://libraries.io/npm/openclaw-termux)[](https://www.160.com/article/12654.html)
        
    - 如果不清楚自己手机的芯片架构，直接下载 `universal` 通用版即可[](https://www.160.com/article/12654.html)。
        
2. **安装并设置**：安装下载的 APK，打开App，点击 **“Begin Setup”** (开始安装)[](https://libraries.io/npm/openclaw-termux)。App会自动下载Ubuntu环境、Node.js和OpenClaw本体[](https://libraries.io/npm/openclaw-termux)[](https://www.160.com/article/12654.html)。
    
3. **配置和启动**：安装完成后，根据指引配置你的大模型 API Key[](https://www.160.com/article/12654.html)。然后回到首页，点击 **“Start Gateway”** (启动网关)即可[](https://libraries.io/npm/openclaw-termux)。
    

### ⌨️ 方案二：Termux 命令行安装（更灵活）

如果你更喜欢命令行，可以通过 npm 来安装。

#### 1. 标准版安装

这是标准的安装方法，适合网络通畅的环境[](https://docs.openclaw.ai/install?utm_source=thedeepview&utm_medium=newsletter&utm_campaign=ai-companies-court-pentagon-anthropic-resists)。

bash

# 1. 安装 Node.js (需要版本 >= 22)[reference:14][reference:15]
pkg update && pkg install nodejs
# 2. 全局安装 OpenClaw
npm install -g openclaw@latest
# 3. 运行初始配置
openclaw onboard --install-daemon
# 4. 启动网关
openclaw gateway run

#### 2. 国内用户优化版 (openclaw-cn-termux)

这个版本专门为国内网络环境优化，将依赖打包成了 npm 包，下载速度更快[](https://www.npmjs.com/package/openclaw-cn-termux)。

- **一键安装脚本（推荐）**：此脚本会在 Termux 中通过 `proot-distro` 创建一个 Ubuntu 环境来进行安装，兼容性更好[](https://www.npmjs.com/package/openclaw-cn-termux)。----------选择此版本
    
    bash
    
    curl -fsSL https://raw.githubusercontent.com/byteuser1977/termux-install-openclaw/main/scripts/install-ubuntu.sh | bash
    
- **手动 npm 安装**：
    
    bash
    
    # 1. 安装 Node.js
    pkg update && pkg install nodejs
    # 2. 使用淘宝镜像安装
    npm install -g openclaw-cn-termux@latest --registry=https://registry.npmmirror.com
    # 3. 运行初始配置
    openclaw-termux onboard
    # 4. 启动网关
    openclaw-termux gateway run
    
    配置文件和标准版不同，位于 `~/.openclaw-cn-termux/` 目录[](https://www.npmjs.com/package/openclaw-cn-termux)。
    

### 🛠️ 从源码构建 (For Developers)

如果你想尝试最新功能或进行二次开发，可以克隆源码自行构建[](https://docs.openclaw.ai/install?utm_source=thedeepview&utm_medium=newsletter&utm_campaign=ai-companies-court-pentagon-anthropic-resists)。

bash

git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install && pnpm build && pnpm ui:build
pnpm link --global
openclaw onboard --install-daemon

### ⚠️ 注意事项

- **后台保活**：在手机系统设置里，为 Termux 或 OpenClaw App 开启“自启动”和“允许后台活动”，防止进程被系统杀死[](https://www.160.com/article/12654.html)。
    
- **Node.js 版本**：确保 Node.js 版本 >= 22[](https://www.npmjs.com/package/openclaw-cn-termux)[](https://docs.openclaw.ai/install?utm_source=thedeepview&utm_medium=newsletter&utm_campaign=ai-companies-court-pentagon-anthropic-resists)。可用 `node --version` 检查。
    
- **命令找不到**：如果安装后提示 `openclaw: command not found`，可能是 npm 的全局 bin 目录未加入环境变量。可以尝试手动添加[](https://docs.openclaw.ai/install?utm_source=thedeepview&utm_medium=newsletter&utm_campaign=ai-companies-court-pentagon-anthropic-resists)：
### Ubuntu具体操作（复制粘贴即可）

1. **进入 Ubuntu 环境**  
    在 Termux 终端执行：
    
    bash
    
    proot-distro login ubuntu
    
    你会看到命令提示符变成 `root@localhost:~#`，说明已经进入了 Ubuntu 容器。
    
2. **在 Ubuntu 内安装 OpenClaw**  
    直接运行日志里给出的脚本（建议用第一行）：
    
    bash
    
    cd ~ && curl -fsSL https://raw.githubusercontent.com/byteuser1977/termux-install-openclaw/main/scripts/install-openclaw.sh | bash
    
    这个脚本会自动在 Ubuntu 中安装 Node.js、pnpm、以及 OpenClaw 本体（国内优化版），耐心等待即可。
    su - openclaw
    
    
3. **安装完成后的配置**  
    脚本跑完后，继续在 Ubuntu 内执行：
    
    bash
    
    openclaw-cn-termux onboard   # 配置 API Key 等
    openclaw-cn-termux gateway   # 启动网关服务
    
4. **访问 Web 界面**  
    启动成功后，在手机浏览器打开 `http://localhost:18789` 即可使用。
    

---

### 💡 补充说明与注意事项

- **关于 `proot-distro`**：它不会真的在你的手机里装一个完整的 Linux 系统，而是通过 **proot** 模拟出一个隔离的 Ubuntu 环境，不需要 root 权限，对手机性能影响很小。
    
- **后台保活**：如果你希望 OpenClaw 在手机熄屏后继续运行，建议在系统设置中为 Termux 开启“自启动”和“不受电池优化限制”，并在 Termux 中执行 `termux-wake-lock`。
    
- **如果安装脚本中途卡住**：国内网络偶尔不稳定，可以尝试换用 `openclaw-cn-termux` 的 npm 安装方式（脚本里已经做了优化），或者手动配置代理。
    
- **退出 Ubuntu**：在 Ubuntu 容器内输入 `exit` 即可返回 Termux 主环境。
### 🚀 让登录变简单的 3 个懒人方案

#### 方案一：一键别名（最推荐）

在 Termux 原生环境（非 Ubuntu 内）配置一个快捷命令，输入一次就自动登录并启动服务。

1. 编辑 Termux 的启动配置文件：
    
    bash
    
    nano ~/.bashrc
    
2. 在文件末尾添加下面这一行（建议直接复制）：
    
    bash
    
    alias openclaw='proot-distro login ubuntu -- bash -c "cd ~ && openclaw-cn-termux gateway"'
    
3. 按 `Ctrl + X`，按 `Y`，再按回车保存。
    
4. 重新加载配置：
    
    bash
    
    source ~/.bashrc
    

以后每次重启 Termux，你只需要在命令行输入 **`openclaw`**，它就会自动进入 Ubuntu 并启动网关，省去了敲 `proot-distro login ubuntu` 的步骤。

#### 方案二：保持后台常驻（不需要重复登录）

如果你不想每次都重新登录，可以让 OpenClaw 一直在后台跑着。

1. 在 Ubuntu 内启动服务后，按 `Ctrl + A` 然后按 `D`（如果能用 screen/tmux）或者直接让它在后台运行。
    
2. **关键在于**：给 Termux 开启唤醒锁，防止进程被系统杀死：
    
    bash
    
    termux-wake-lock
    
3. 这样只要你不主动关闭 Termux 后台，切回来时你的 Ubuntu 会话和环境都还在，不用重新登录。
    

#### 方案三：完全开机自启（进阶）

配合 Termux 的 `termux-boot` 插件，可以设置手机开机时自动启动 Termux 并执行登录 Ubuntu 的命令。这个设置稍复杂一点，但如果你的需求很稳定，可以让它像智能音箱一样随时待命。

---

### 💡 简单总结

- **直接用**：每次输 `proot-distro login ubuntu`，然后输命令（这是最笨的方法）。
    
- **懒人用**：配置别名后，每次只需输 **`openclaw`**，回车，搞定。
    
- **极致懒**：保持后台不关，配上唤醒锁，一次登录，用到手机关机。