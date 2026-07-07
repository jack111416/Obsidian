1|User uses WeChat (微信 ClawBot) as messaging platform via Hermes Gateway.
2|§
3|User prefers StepFun Step-3.5-Flash model on Nvidia provider; model string: nv:stepfun-ai/step-3.5-flash.
4|§
5|User environment: Android Termux. custom_providers includes nv (Nvidia) and aliyuncs (Alibaba Cloud). .env contains GLM_API_KEY and GLM_BASE_URL (legacy).
6|§
7|重要教训：即使是为了修复问题，修改配置文件或重启服务前也必须先征得用户明确同意。用户坚持"不要自动切换模型，任何配置更改前必须先询问"。之前未经同意修改 config.yaml 并重启网关，违反了这一偏好。
8|§
9|Git installed v2.54.0, GitHub latency ~113ms. Obsidian vault: ~/ObsidianVault/, Git remote: github.com:jack111416/Obsidian.git (SSH)
10|§
11|User prefers visual knowledge bases combining screenshots + transcripts. Expects B站 video extraction workflow: metadata → audio download → ASR → markdown chunks → summary → visual elements.
12|§
13|Primary: nv:stepfun-ai/step-3.5-flash. Local provider 127.0.0.1:8080 defunct. Ollama 127.0.0.1:11434 working.
14|§
15|修改 config.yaml 之前必须先运行备份脚本 ~/.hermes/scripts/config_backup.py。这在每次需要修改模型配置、备用模型列表或其他关键设置时自动执行。
16|§
17|EasyTier 重启流程（Android Termux 关键坑）：1) pkill -f easytier-core 2) sudo ip link del tun0 tun1 tun2 3) cd ~/easytier-linux-aarch64 && sudo ./easytier-core -c ~/easytier-config.toml > easytier.log 2>&1 &（后台启动）。原因：pkill 后内核不自动删除 TUN 接口，再启动会递增创建 tun1/tun2/tun3，导致端口转发绑定到错误接口。必须先手动 ip link del 清理残留接口。