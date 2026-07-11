OpenClaw官网：[https://openclaw.ai/](https://openclaw.ai/)

官方WSL方式教程：[https://openclaw.cc/platforms/windows](https://openclaw.cc/platforms/windows)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/53337186/1775745142321-7e8ed68c-a92a-4b8c-8e73-8b7c51cbbc49.png)

## 1，激活Linux子系统
开启虚拟机支持

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/53337186/1775745794727-c4c20d57-8551-4fec-aa8c-0ecb9643cfd3.png)

以管理员方式运行powershell

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/53337186/1775745884710-5bb3e686-7943-4e66-89e1-65aa2c1b91f4.png)

```shell
# 官方提供的分步安装
wsl --install
# Or pick a distro explicitly:
wsl --list --online
wsl --install -d Ubuntu-24.04

# 卡0%解决方案
wsl --install -d Ubuntu --web-download
```

<font style="color:#DF2A3F;">貌似我们赶上了目标官网出现问题，Ubuntu一直拉取不下来，这里我们通过清华大学开源软件镜像站手动下载</font>

官网：[https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/noble/](https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/noble/)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/53337186/1775748710774-856baf69-c992-46d7-a667-1df4cd9112c0.png)

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/53337186/1775748776442-7e121604-6069-43b2-b675-021df522c653.png)

```powershell
# 管理员运行powershell
wsl --import Ubuntu-24.04 C:\WSL\Ubuntu2404 "C:\WSL\ubuntu-24.04.4-wsl-amd64.wsl"
```

启动linux镜像

```powershell
wsl -d Ubuntu-24.04
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/53337186/1775748889884-2cc0b475-6a1a-42e3-91dd-9af005aecd64.png)

```bash
# 创建新用户
useradd -m -s /bin/bash openclaw
# 设置密码
passwd openclaw
# 加入管理员组
usermod -aG sudo openclaw
```

## 2，安装OpenClaw
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/53337186/1775749486952-5d966f1a-a217-4a76-bf7b-7d924e876d47.png)

这里等待时间非常之漫长~看到如下界面表示安装成功，见下一章节初步配置。

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/53337186/1775750988997-5454aaa8-5287-4a26-8759-2cedf7b2d7e9.png)

## 3，初始配置说明
```shell
◇  I understand this is personal-by-default and shared/multi-user use requires lock-down. Continue?
│  Yes
│
◇  Setup mode
│  QuickStart
│
◇  QuickStart ─────────────────────────╮
│                                      │
│  Gateway port: 18789                 │
│  Gateway bind: Loopback (127.0.0.1)  │
│  Gateway auth: Token (default)       │
│  Tailscale exposure: Off             │
│  Direct to chat channels.            │
│                                      │
├──────────────────────────────────────╯
│
◇  Model/auth provider
│  Skip for now
◇  Filter models by provider
│  All providers
│
◇  Default model
│  Keep current (default: openai/gpt-5.4)
◇  Select channel (QuickStart)
│  Skip for now
◇  Search provider
│  Skip for now
◇  Configure skills now? (recommended)
│  Yes
│
◇  Install missing skill dependencies
│  Skip for now
│
◇  Set GOOGLE_PLACES_API_KEY for goplaces?
│  No
│
◇  Set NOTION_API_KEY for notion?
│  No
│
◇  Set OPENAI_API_KEY for openai-whisper-api?
│  No
│
◇  Set ELEVENLABS_API_KEY for sag?
│  No
◇  Enable hooks?
│  🚀 boot-md, 📎 bootstrap-extra-files, 📝 command-logger, 💾 session-memory

◇  Control UI ─────────────────────────────────────────────────────────────────────╮
│                                                                                  │
│  Web UI: http://127.0.0.1:18789/                                                 │
│  Web UI (with token):                                                            │
│  http://127.0.0.1:18789/#token=2a6a834f43ef1046858029463e86251f6118c973d281c7b3  │
│  Gateway WS: ws://127.0.0.1:18789                                                │
│  Gateway: not detected (gateway closed (1006): )                                 │
│  Docs: https://docs.openclaw.ai/web/control-ui                                   │
│                                                                                  │
├──────────────────────────────────────────────────────────────────────────────────╯
```

这里我们安装过程中npm环境全局自动配置失败了，手动修复（如果遇到，就是页面无法访问）

```bash
npm config get prefix		# 输出大概率是/home/openclaw/.npm-global
echo 'export PATH="$HOME/.npm-global/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
openclaw --version

# 启动openclaw Gateway
openclaw gateway run
```

这里我们成功进入到主界面

<!-- 这是一张图片，ocr 内容为： -->
![](https://cdn.nlark.com/yuque/0/2026/png/53337186/1775752735014-43155ba5-a004-4073-bb25-75015ba57b12.png)





















