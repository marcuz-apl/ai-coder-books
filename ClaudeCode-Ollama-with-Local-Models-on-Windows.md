# Claude Code - Ollama with Local Models in Windows

by Marcus Zou | 20 April 2026 | Source: https://claude.ai



## Intro

Claude Code is an Agent framework, not the AI model, then it can be installed any way.

Claude Code can integrate the Claude official AI model, as well as all Chinese AI Models running locally.

The best AI Agent framework is Claude Code by far.



## Step 1 - Install Claude Code

**Windows - Install with PowerShell**

Pre-install Git first:

```powershell
winget install git.git
```

Then install Claude Code:

```shell
irm https://claude.ai/install.ps1 | iex
```

**Windows - Install with winget**

```shell
winget install Anthropic.ClaudeCode
```

In whatever case, you have to get Claude verified.

Verify: type in -

```shell
claude --version
## shall be version number
```



## Step 2 - Install Ollama on Windows

Very easy to use PowerShell:

```powershell
# install
irm https://ollama.com/install.ps1 | iex
# version
ollama -v
```

Go to https://ollama.com/search to take a look at those models, all free.

Then pull down some models:

```powershell
ollama pull qwen3.6:latest			## 23 GB
ollama pull qwen3-coder:30b			## 18 GB
ollama pull qwen3-coder:480b-cloud	## - 
ollama pull gemma4:31b				## 19 GB
ollama pull gemma4:31b-cloud		## 19 GB
ollama pull codellama:13b			## 7.4 GB
ollama pull codellama:34b			## 19 GB
```

Give a try:

```shell
ollama run gemma4:31b
```

Ollama server is served at http://localhost:11343/v1



## Step 2B - Alternatively Install CC Switch

China AI Models are very powerful as well, say GLM-5.1, which is very close to Claude Opus 4.6; If no GLM coding plan, MiniMax M2.7 and Kimi K2.5 are also very great.

Strongly recommend to use CC Switch to switch AI Models.

**Install CC Switch in macOS**

```shell
brew tap farion1231/ccswitch
brew install --cask cc-switch
```

**Install CC Switch in Windows**

Go to https://github.com/farion1231/cc-switch/releases to download the installer pack, then install it.

**Configure CC-Switch**

Open CC Switch, click "Claude" column, Add new model, fill in:

- API Key
- Select your model
- The rest parts will be filled in by CC Switch automatically.



## Step 3 - Configure and Spin up Claude Code

Set Environment Variables for Claude Code in PowerShell:

```powershell
$env:ANTHROPIC_BASE_URL="http://localhost:11434"
$env:ANTHROPIC_AUTH_TOKEN="ollama"
$env:ANTHROPIC_API_KEY=""
```

Or add 3 lines into the config file: `~/.claude/settings.json`:

```shell
nano ~/.claude/settings.json
```

The contents belike:

```json
{
  "env": {
    "CLOUD_ML_REGION": "global",
    "ANTHROPIC_BASE_URL": "http://localhost:11434",
    "ANTHROPIC_AUTH_TOKEN": "ollama",
    "ANTHROPIC_API_KEY": ""
  },
  "autoUpdatesChannel": "latest",
  "theme": "dark",
  "skipDangerousModePermissionPrompt": true
}
```



**In case of you prefer Anthropic Cloud-based model**

If you prefer to use Anthropic model, Opus 4.7, you can go to the project folder and type in:

```shell
cd claudeDemo
claude init
```

As such you will be diverted to selecting the official model of Anthropic, instead of the local ones. 

They are expensive, but very deserved.



Go into the Project folder and Run command from Terminal:

```shell
mkdir claudeDemo
cd claudeDemo

claude --model qwen3.6
```

When Developing something, use this code below will save your day quite a bit, trust me.

```shell
claude --dangerously-skip-permisions --model qwen3-coder:30b
# or 
ollama launch claude
## Then you got a chance to select the model you installed.
```



## Step 4 - Write your CLAUDE.md

This step is very critical.

#### What's CLAUDE.md

Without CLAUDE.md, Claude Code is like a newbie, not knowing anything, no style, no boundary, no bottom line, asking you all sorts of Qs again and again.

With CLAUDE.md, the Claude Code will know your needs deeply, working as collaborator, knowing what to do, what not to do. 

#### CLAUDE.md structure

**Global CLAUDE.md**: located at `~/.claude/CLAUDE.md`. Once configured, it will be loaded for every projects. It's about who you are, your work principles. Please note: Ensure it's less than 80 lines.

**Project CLAUDE.md**: located at the root directory of each project; effective only when the project is loaded. This md file doesn't need your effort, because when you chat with Claude Code, it will draft one for you.



## Coding Challenge: a Dinosaur Game

```text
Please use HTML_JavaScript to make Chrome-based offline dinosaur game in a single index.html file, Using space bar to jump, on a land with cactus barrier; starting from left side; It changes to Game-over if hitting a barrier, after which, score shall be displayed, plus a Restart button pops up; The display shall be in beautiful pixels.
```



## Benchamark Models

If installing models with Ollama, we can use `ollama-benchmark` to monitor the token per second.

```shell 
pip install ollama-benchmark
## Or
pip install llm-benchmark
```



## The End