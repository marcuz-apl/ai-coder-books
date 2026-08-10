# Use GLM 5.2 in Claude Code for Free

https://www.youtube.com/watch?v=n2JlvhpKHgk



## How it Works

![Roadmap](assets\20260708-001.png)

This article shows you how to connect Z.ai's powerful GLM-5.2 model with Claude Code using NVIDIA NIM and Jan AI, without paying for Claude API usage.

Whether you're a developer, AI enthusiast, or just looking for a powerful free coding model, this guide will help you get started in just a few minutes. 

Resources: 
🔹 Claude Code https://claude.ai/code
🔹 Jan AI https://jan.ai
🔹 NVIDIA NIM https://build.nvidia.com



## Steps

1- Install Claude Code

```shell
# Windows PowerShell
irm https://claude.ai/install.ps1 | iex
## or via winget
winget install Anthropic.ClaudeCode

# macOS/Linux
curl -fsSL https://claude.ai/install.sh | bash
```

2- Install Jan AI

```shell
# Windows
wget https://github.com/janhq/jan/releases/download/v0.8.3/Jan_0.8.3_x64-setup.exe
# macOS
wget https://github.com/janhq/jan/releases/download/v0.8.3/Jan_0.8.3_universal.dmg
# Linux
wget https://github.com/janhq/jan/releases/download/v0.8.3/Jan_0.8.3_amd64.deb
```

Install it, don't download the default LLM model from Jan.AI though.

3- Create and save the API Key if Nvidia NIM Models

Go to https://build.nvidia.com, login, create API Key: 

(**mine**) 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

4- Setup in Jan AI: Click "**Settings**" -> 

​	4.1 "REMOTE" Part: Scroll down to "NVIDIA NIM", type in the API Keys you received. Refresh and scroll down to "**z-ai/glm-5.2**".

​	4.2 "INTEGRATIONS" Part: click "Clause Code", then select "Large/Medium/Small model" to "**z-ai/glm-5.2**". Save & Enable.

​	4.3 "Local API Server" part: make sure the Local API Server is running: http://127.0.0.1:1337/v1 and enable "Auto Start".

## License

MIT License