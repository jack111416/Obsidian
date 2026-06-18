### 🚀 如何在当前的 Ubuntu 环境中安装 Hermes

Hermes 的安装非常简单，推荐使用官方的一键安装脚本[](https://cloud.tencent.com.cn/developer/article/2683024)[](https://cloud.tencent.com.cn/developer/article/2678643)。

1. **进入 Ubuntu 环境**：  
    在 Termux 终端执行：
    
    bash
    
    proot-distro login ubuntu
    
2. **执行一键安装命令**：  
    在 Ubuntu 的命令行中，运行以下命令之一：
    
    - **国际官方源（通用）**[](https://cloud.tencent.com.cn/developer/article/2678643)[](https://cloud.tencent.com.cn/developer/article/2675116):
        
        bash
        
        curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
        
    - **国内镜像源（推荐，速度更快）**[](https://cloud.tencent.com.cn/developer/article/2675116):
        
        bash
        
        curl -fsSL https://res1.hermesagent.org.cn/install.sh | bash
        
3. **激活环境并验证**：  
    安装脚本会自动处理 Python、Node.js 等所有依赖。完成后，刷新环境变量并验证安装：
    
    bash
    
    source ~/.bashrc
    hermes doctor   # 检查依赖是否完整[reference:9]
    
4. **配置并启动**：  
    安装完成后，需要进行基本配置：
    
    bash
    
    hermes setup    # 配置你的大模型 API Key[reference:10]
    
    之后就可以使用了，例如：
    
    bash
    
    hermes shell    # 进入命令行交互模式
    