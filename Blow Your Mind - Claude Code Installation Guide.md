# Blow Your Mind: Claude Code - Install Guide

by Marcus Zou | 20 April 2026 | Source: https://claude.ai



## Intro

Claude Code is an Agent framework, not the AI model, then it can be installed any way.

Claude Code can integrate the Claude official AI model, as well as all Chinese AI Models.

The best AI Agent framework is Claude Code by far.



## Scenarios

- Mac or Windows
- With Magic or Plain Internet



## Step 1 - Install Claude Code framework

**macOS - install with bash**

Open terminal and type:

```shell
curl -fsSL https://claude.ai/install.sh | bash
```

It may prompt to add the PATH, please do so using `echo` command.

Verify: type in -

```shell
claude --version
## shall be version number
```

**macOS - brew it**

use `homebrew` to install:

```shell
# Install Homebrew
/bin/bash -c "(curl -fsSL https://raw.hithubusercontent.com/Homebrew/install.sh/HEAD/install.sh)"
```

use Homebrew to install Claude Code:

```shell
brew install --cask claude-code@latest
claude --version
```



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



## Step 2 - Install CC Switch

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



## Step 3 - Spin up Claude Code

Go into the Project folder and Run command from Terminal:

```shell
claude
```

When Developing something, use this code below will save your day quite a bit, trust me.

```shell
claude --dangerously-skip-permisions
```



## Step 4 - Write your CLAUDE.md

This step is very critical.

#### What's CLAUDE.md

Without CLAUDE.md, Claude Code is like a newbie, not knowing anything, no style, no boundary, no bottom line, asking you all sorts of Qs again and again.

With CLAUDE.md, the Claude Code will know your needs deeply, working as collaborator, knowing what to do, what not to do. 

#### CLAUDE.md structure

**Global CLAUDE.md**: located at `~/.claude/CLAUDE.md`. Once configured, it will be loaded for every projects. It's about who you are, your work principles. Please note: Ensure it's less than 80 lines.

**Project CLAUDE.md**: located at the root directory of each project; effective only when the project is loaded. This md file doesn't need your effort, because when you chat with Claude Code, it will draft one for you.



## The End