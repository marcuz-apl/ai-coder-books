# Claude Code + Ollama or LM Studio + Local Model on Mac/Linux/Windows

Marcus Zou | 20 April 2026 | Source: https://claude.ai

[TOC]



## Intro

Claude Code is an Agent framework, not the AI model, then it can be installed without subscriptions.

- Claude Code can integrate the Claude official AI model, as well as all local models.

- The best AI Agent framework is Claude Code by far.

- Token fee is soaring to sky.



## 1A- Install Claude Code

```shell
# Windows - via PowerShell or winget
irm https://claude.ai/install.ps1 | iex
## winget install Anthropic.ClaudeCode

# macOS/Linux - via bash or npm
curl -fsSL https://claude.ai/install.sh | bash
## npm nstall -g @anthropic-ai/claude
## macOS Only
## brew install --cask claude-code@latest

# Add the env in macOS
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc
# Add the env in Linux
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc && source ~/.bashrc

# Check version
claude -v
```



## 1B- Install Claude Desktop

For Windows, just download: https://claude.ai/api/desktop/win32/x64/setup/latest/redirect?utm_source=claude_code&utm_medium=docs and install it.

For macOS, just download: https://claude.ai/api/desktop/darwin/universal/dmg/latest/redirect?utm_source=claude_code&utm_medium=docs and install it.

For Linux, install the beta version at this moment:

```shell
# Download the Claude-Desktop installer
curl -fLO "https://downloads.claude.ai/claude-desktop/apt/stable/$(curl -s "https://downloads.claude.ai/claude-desktop/apt/stable/dists/stable/main/binary-$(dpkg --print-architecture)/Packages" | grep '^Filename: pool/main/c/claude-desktop/claude-desktop_' | sort -V | tail -n 1 | cut -d' ' -f2)"
# Run the installer to install
sudo apt install ./claude-desktop_*.deb
# Clean
rm claude-desktop_*.deb

## Uninstall?
sudo apt remove claude-desktop
```





## 2A- Install LM Studio CLI and Desktop

```shell
# LM Studio CLI
## Windows
irm https://lmstudio.ai/install.ps1 | iex
## Mac/Linux
curl -fsSL https://lmstudio.ai/install.sh | bash
## to start the daemon, run
lms daemon up
## open a new terminal and check the version
lms -v

# LM Studio Desktop
## Windows
wget https://lmstudio.ai/download/latest/win32/x64
#### Then install it
## macOS
wget https://lmstudio.ai/download/latest/darwin/arm64
#### Then install it
## Linux
wget https://installers.lmstudio.ai/linux/x64/0.4.20-1/LM-Studio-0.4.20-1-x64.deb
sudo dpkg -i LM-Studio-0.4.20-1-x64.deb
### Clean
rm LM-Studio-*.deb
```



## 2B- Install Ollama

```shell
# Windows
irm https://ollama.com/install.ps1 | iex

# Mac/Linux
curl -fsSL https://ollama.com/install.sh | sh

# version
ollama -v
```



## llmfit - discover which model fits

```shell
# Windows
scoop install llmfit

# macOS/Linux
curl -fsSL https://llmfit.axjns.dev/install.sh | sh
## brew install AlexsJones/llmfit/llmfit
## Verify
llmfit --version

# Run the app
llmfit
```



## 3A- Pull down models with LMS Studio

```shell
## Smaller model in macOS (MacBook Air M5 - 16GB RAM)
lms get google/gemma-4-e4b
## Bigger model on Dell Precision 7820 Tower - 256GB RAM + 16G VRAM x 2
lms get google/gemma-4-31b
lms get google/gemma-4-26b-a4b
```



## 3B- Pull down models with Ollama

```shell
## ------- Google Family ---------
ollama pull gemma4:12b				## 7.6 GB | 256K | Text,Image
ollama pull gemma4:26b				## 18 GB | 256K | Text,Image
ollama pull gemma4:31b-cloud		## ----- | 256k | Text,Image
## ------- Alibaba Family --------
ollama pull qwen3.6:35b				## 23 GB | 256K	| Text,Image
ollama pull qwen3-coder:30b			## 19 GB | 256K	| Text
ollama pull qwen3-coder:480b-cloud	## ----- | 256K	| Text
## ------- Deepseek Family -------
ollama pull deepseek-v4-pro:cloud	## ----- | 1M | Text
ollama pull deepseek-v4-flash:cloud	## ----- | 1M | Text
## ------- Nvidia Family -------
ollama pull nemotron-3-nano:30b 	## ----- | 1M | Text

## -------- Special Editions ------
## 6.8GB | 1M | text,image
ollama pull hf.co/empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF:Q4_K_M 
## 6.8GB | 1M | text,image
ollama pull hf.co/yuxinlu1/gemma-4-12B-agentic-fable5-composer2.5-v2-3.5x-tau2-GGUF:Q4_K_M
ollama pull xentriom/gemma-4-12B-agentic-fable5-composer2.5-v2:Q4_K_M
```



## 4A- Start LM Studio Server

```shell
# Start the Server
lms server start --port 1234
## Success! Server is now running on port 1234

# Check the status
lms status
## Server: ON (port: 1234) 
## loaded Models
##   . google/gemma-4-e4b - 6.33 GB
```

LMS server is served at http://localhost:1234



## 4B- Load up Ollama Models

```shell
## Ollama model
ollama run gemma4:31b
## Success! Server is now running on port 11434
## Huggingface Model
ollama run hf.co/yuxinlu1/gemma-4-12B-agentic-fable5-composer2.5-v2-3.5x-tau2:Q4_K_M
```

Ollama server is served at http://localhost:11434



## 5- Divert ANTHROPIC default path to Local Model

Set Environment Variables for Claude Code:

```powershell
# Windows - PowerShell
$env:ANTHROPIC_BASE_URL="http://localhost:11434"
$env:ANTHROPIC_AUTH_TOKEN="ollama"
$env:ANTHROPIC_API_KEY=""
```

**Note: Please do not set both AUTH_TOKEN and API_KEY! Leave one null.**

```shell
# macOS/Linux
export ANTHROPIC_BASE_URL="http://localhost:1234"
export ANTHROPIC_AUTH_TOKEN="lmstudio"
# export ANTHROPIC_API_Key="lmstudio"
```

The settings above is valid for current Terminal session only.



To enable the settings permanently, add the 3 lines above into `~/.claude/settings.json` or  `~/.zshrc` or `~/.bashrc`.

```shell
## Windows
nano ~/.claude/settings.json
## macOS
nano ~/.zshrc
## Linux
nano ~/.bashrc
```

The contents of setings.json belike:

```text
"ANTHROPIC_BASE_URL": "http://localhost:11434",
"ANTHROPIC_AUTH_TOKEN": "ollama",
"ANTHROPIC_API_KEY": ""
```

The contents of .bashrc belike:

```text
ANTHROPIC_BASE_URL="http://localhost:11434" 
ANTHROPIC_AUTH_TOKEN="ollama" 
ANTHROPIC_API_KEY=""
```

Restart the terminal to make it effective.



## 6A- Launch the default Claude models (official way)

```shell
## Create a folder and launch up
mkdir -p ~/projects/claudedemo
cd ~/projects/claudeDemo
claude init
```

As such you will be diverted to **selecting the official model of Anthropic**, instead of the local ones. 

They are expensive, but very deserved.



## 6B- Launch the local model with ollama

```shell
## create a folder
mkdir -p ~/projects/claudeDemo
cd ~/projects/claudeDemo

## macOS case
ollama launch claude --model google/gemma-4-e4b
## load the monster model into the beast machine: Dell Precision 7820
ollama launch claude --model google/gemma-4-12b
## use this code below will save your day quite a bit, trust me
ollama launch claude --dangerously-skip-permisions --model xentriom/gemma-4-12B-agentic-fable5-composer2.5-v2:Q4_K_M

## Exit the claude
/exit
```



## 7- Change the default context length

```shell
# unload all models
lms unload
# Reload the model with specified context length
lms load google/gemma-4-e4b --context-length 40960

# Run Claude Code + Gemma4 model
claude --model google/gemma-4-e4b
```



## 8- Write your CLAUDE.md

This step is very critical.

### What's CLAUDE.md

Without CLAUDE.md, Claude Code is like a newbie, not knowing anything, no style, no boundary, no bottom line, asking you all sorts of Qs again and again.

With CLAUDE.md, the Claude Code will know your needs deeply, working as collaborator, knowing what to do, what not to do. 

### CLAUDE.md structure

**Global CLAUDE.md**: located at `~/.claude/CLAUDE.md`. Once configured, it will be loaded for every projects. It's about who you are, your work principles. Please note: Ensure it's less than 80 lines.

**Project CLAUDE.md**: located at the root directory of each project; effective only when the project is loaded. This md file doesn't need your effort, because when you chat with Claude Code, it will draft one for you.

Here is my example CLAUDE.md

```markdown
# Project Guidelines

## Tech Stack
- Frontend: Next.js 14, TypeScript, Tailwind CSS
- Database: PostgreSQL with Prisma ORM

## Coding Conventions
- Use `const` over `let`. Never use `var`.
- Use named exports.
- File names: kebab-case for components, camelCase for utilities.

## Rules
- DO NOT add new npm packages without asking.
- RUN `npm run test` before committing changes.

## Commands
- Dev server: `npm run dev`
- Tests: `npm run test`
```

### Best Practices

- **Be Specific:** Instead of "write clean code," write "use JSDoc for all public functions".
- **Use Progressive Disclosure:** For large projects, don't put everything in one file. Mention other docs, such as `See @README.md for project overview`.
- **Keep it Current:** If you notice Claude making the same mistake repeatedly, add a rule to the `CLAUDE.md` file to prevent it.
- **Use `CLAUDE.local.md`:** For personal preferences that shouldn't be shared with your team via git, create a `CLAUDE.local.md` file and add it to your `.gitignore`.
- **Use Hierarchy:** You can add `CLAUDE.md` files in subdirectories for rules specific to that folder



## 9- Coding Challenge: a dinosaur game

Please note, when writing the prompt, better to write all requirements in one shot to save tokens.

For instance, I typed in:

```text
Please use HTML + JavaScript to make Chrome-based offline dinosaur game in a single index.html file, Using space bar to jump, on a land with cactus barriers; starting from left side; It changes to Game-over if hitting a barrier, after which, score shall be displayed, plus a Restart button pops up; The display shall be in beautiful pixels. Eventurally save this file to the current project folder.
```

I renamed the `intex.html` to `./claudeDemo/dino-game-gemma4.html`, it's not playable, to be honest.



## Benchmark Models

If installing models with Ollama, we can use `ollama-benchmark` to monitor the token per second.

```shell 
pip install ollama-benchmark
## Or
pip install llm-benchmark
```



## The End
