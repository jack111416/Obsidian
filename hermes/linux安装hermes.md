### 方案一：一键安装脚本（最推荐）

这是针对 Linux 环境最快捷的方式[](https://www.ithome.com/0/955/186.htm)。考虑到网络问题，建议优先使用国内镜像源：

bash

# 使用国内镜像源（推荐，速度更快）
curl -fsSL https://res1.hermesagent.org.cn/install.sh | bash

如果国内镜像源不可用，可以尝试官方源：

bash

# 官方源
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash

安装脚本会自动处理 Python 环境、依赖包等[](https://www.ithome.com/0/957/639.htm)。安装完成后，刷新环境变量并验证是否成功：

bash

source ~/.bashrc  # 或者 source ~/.zshrc
hermes --version  # 应该会显示版本号，如 v0.8.0+

### 🐍 方案二：使用 pip 安装

如果你的环境已配置好 Python，也可以用 pip 安装[](https://www.ithome.com/0/957/639.htm)[](https://www.geekpark.net/news/365380)：

bash

pip install hermes-agent
hermes postinstall  # 执行可选的后安装配置

### 🛠️ 方案三：源码安装（适合开发者）

这种方式更灵活，适合想要尝鲜或二次开发的用户[](https://www.ithome.com/0/957/639.htm)[](https://www.geekpark.net/news/365380)：

bash

git clone https://github.com/NousResearch/hermes-agent.git
cd hermes-agent
python3.11 -m venv venv
source venv/bin/activate
pip install -e ".[all]"

之后可以创建软链接方便全局调用：

bash

mkdir -p ~/.local/bin
ln -sf "$(pwd)/venv/bin/hermes" ~/.local/bin/hermes

### 💻 方案四：使用 Ollama 本地部署（完全离线）

如果你想完全在本地运行模型，不依赖任何 API，可以结合 Ollama 使用[](https://hermes-agent.nousresearch.com/docs/guides/local-ollama-setup)。

1. **安装 Ollama**：
    
    bash
    
    curl -fsSL https://ollama.com/install.sh | sh
    
2. **拉取一个模型**（根据你的硬件配置选择）[](https://hermes-agent.nousresearch.com/docs/guides/local-ollama-setup)：
    
    bash
    
    # 例如，拉取一个轻量级的 3B 模型
    ollama pull llama3.2:3b
    
3. **配置 Hermes**：运行 `hermes setup`，在提示选择提供商时，选 **`Custom Endpoint`**，然后设置：
    
    - **Base URL**: `http://localhost:11434/v1`[](https://hermes-agent.nousresearch.com/docs/guides/local-ollama-setup)
        
    - **API Key**: 留空或填 `no-key`[](https://hermes-agent.nousresearch.com/docs/guides/local-ollama-setup)
        

### ⚙️ 配置与启动

安装完成后，需要配置 Hermes 才能使用：

1. **运行配置向导**：
    
    bash
    
    hermes setup
    
    这个命令会引导你完成设置[](https://www.ithome.com/0/955/186.htm)[](https://www.geekpark.net/news/365380)。
    
2. **选择模型提供商**：
    
    - **国内用户**：推荐选择 **Kimi (Moonshot)**，网络稳定，体验好[](https://www.ithome.com/0/955/186.htm)[](https://cloud.tencent.com.cn/developer/article/2675116)。
        
    - **海外用户**：可选择 **Nous Portal**, **OpenRouter**, **OpenAI** 等[](https://cloud.tencent.com.cn/developer/article/2675116)。
        
    - **本地模型**：如果你按方案四安装了 Ollama，选择 **`Custom Endpoint`**。
        
3. **输入 API 密钥**：如果选择了云服务商，需要输入你的 API 密钥[](https://www.ithome.com/0/955/186.htm)。
    
4. **启动 Hermes**：一切就绪后，在终端输入以下命令即可开始使用：
    
    bash
    
    hermes