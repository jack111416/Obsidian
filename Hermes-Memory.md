User uses WeChat (微信 ClawBot) as messaging platform via Hermes Gateway.
§
User prefers StepFun Step-3.5-Flash model on Nvidia provider; model string: nv:stepfun-ai/step-3.5-flash.
§
User environment: Android Termux. custom_providers includes nv (Nvidia) and aliyuncs (Alibaba Cloud). .env contains GLM_API_KEY and GLM_BASE_URL (legacy).
§
重要教训：即使是为了修复问题，修改配置文件或重启服务前也必须先征得用户明确同意。用户坚持"不要自动切换模型，任何配置更改前必须先询问"。之前未经同意修改 config.yaml 并重启网关，违反了这一偏好。
§
Git installed v2.54.0, GitHub latency ~113ms. Obsidian vault: ~/ObsidianVault/, Git remote: github.com:jack111416/Obsidian.git (SSH)
§
User prefers visual knowledge bases combining screenshots + transcripts. Expects B站 video extraction workflow: metadata → audio download → ASR → markdown chunks → summary → visual elements.
§
Primary: nv:stepfun-ai/step-3.5-flash. Local provider 127.0.0.1:8080 defunct. Ollama 127.0.0.1:11434 working.
§
修改 config.yaml 之前必须先运行备份脚本 ~/.hermes/scripts/config_backup.py。这在每次需要修改模型配置、备用模型列表或其他关键设置时自动执行。
§
EasyTier 启动：cd ~/easytier-linux-aarch64 && sudo nohup ./easytier-core -c ~/easytier-config.toml > easytier.log 2>&1 &
§
WebUI 启动：cd ~/hermes-webui && ./ctl.sh start