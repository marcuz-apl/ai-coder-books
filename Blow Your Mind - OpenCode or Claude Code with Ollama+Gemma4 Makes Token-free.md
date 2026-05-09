# Blow Your Mind: OpenCode or Claude Code with Ollama+Gemma4 Makes Token-Free

 

[TOC]

## Mindset

- Have to work in Windows env daily, and
- Love Vibe Coding within Visual Studio Code, and
- Don't wanna go dual-booting the workstation, and
- Wanna cut cost of tokens to Zero



## Approaches

- Install `VSCode` as Easy-to-Handle IDE
- Install `Claude Code` to manage Vibe coding process
- Install `ollama` - taking care of local LLM management
- Grab `Gemma4` LLM Model to make token-free coding



## Operations



### 1- Install VS Code

Download `VS Code` from Microsoft site and install it.

```shell
# Windows
wget https://vscode.download.prss.microsoft.com/dbazure/download/stable/8b640eef5a6c6089c029249d48efa5c99adf7d51/VSCodeUserSetup-x64-1.119.0.exe
# macOS
wget https://vscode.download.prss.microsoft.com/dbazure/download/stable/8b640eef5a6c6089c029249d48efa5c99adf7d51/VSCode-darwin-universal.dmg
# Debian/Ubuntu
wget https://vscode.download.prss.microsoft.com/dbazure/download/stable/8b640eef5a6c6089c029249d48efa5c99adf7d51/code_1.119.0-1778006717_amd64.deb
dpkg -i code_1.119.0-1778006717_amd64.deb
# Fedora/RHEL/Rocky
wget https://vscode.download.prss.microsoft.com/dbazure/download/stable/8b640eef5a6c6089c029249d48efa5c99adf7d51/code-1.119.0-1778006763.el8.x86_64.rpm
rpm -ivh code-1.119.0-1778006763.el8.x86_64.rpm
```



### 2- Install Git

Download Git installer from https://git-scm.com/install/ and install Git, or simply:

```shell
# Windows
winget install --id Git.Git -e --source winget
# macOS
brew install git
# Debian/Ubuntu
apt install git
# Fedora/RHEL/Rocky
dnf install git
```



### 3- Install Vibe Coding Agents

Install `OpenCode` and/or `Claude Code` as vibe coding agent.

```shell
# Install OpenCode
curl -fsSL https://opencode.ai/install | bash
## npm i -g opencode-ai

# Install Claude Code
curl -fsSL https://claude.ai/install | bash
## npm i -g @anthropic-ai/claude-code
```



### 4- Install `Ollama`

```shell
# macOS/Linux
curl -fsSL https://ollama.com/install.sh | sh
# Windows - PowerShell
irm https://ollama.com/install.ps1 | iex
####
#### Nvidia GPU detected
#### The Ollama API is now available at 127.0.0.1:11434
####

# Check the version
ollama -v
```



### 5- Install `llmfit` 

very simple process as below:

```shell
# Install llmfit
curl -fsSL https://llmfit.axjns.dev/install.sh | sh
# Run llmfit to select the best llm model
llmfit
# Since WSL2 communicate with and share the hardware with the host,
# The best models will be listed.
# Select the model with a higher score
```

For my powerful Dell Precision 7820 Tower, with 256Gb RAM, 8TB HDD, NVIDIA Quadro P5000 (16GB VRAM) x2, I am very comfortable to pick up some super powerful LLM models.

```shell
# Qwen Family
Qwen3.5-35B-A3B
Qwen3.6-27B
Qwen3-14B
Qwen2.5-Coder-14B-Instruct
# Google family
gemma-4-31B-it
gemma-4-E4B-it
# Microsoft
phi-4-reasoning
```



## 6- LLM Naming Convention: `ollama` vs.` llmfit`

By the way - the format on `ollama` site slightly differ from `llmfit`'s output

```shell
# Gemma Family
ollama pull gemma4:e4b				## Size:9.6GB, Context:128k, Input:text,image
ollama pull gemma4:31b				## Size:20GB, Context:256k, Input:text,image
ollama pull gemma4:31b-cloud		## Size:----, Context:256k, Input:text,image

# Qwen Family
ollama pull qwen3:14b
ollama pull qwen3-coder:30b			## Size:19GB, Context:256k, Input:text
ollama pull qwen3-coder:480b-cloud	## Size:----, Context:256k, Input:text
ollama pull qwen3-coder-next:cloud	## Size:----, Context:256k, Input:text
ollama pull qwen3-vl:8b				## Size:6.1GB, Context:256k, Input:text,image
ollama pull qwen3-vl:30b			## Size:20GB, Context:256k, Input:text,image
ollama pull qwen3-vl:235b-cloud		## Size:----, Context:256k, Input:text,image

ollama pull qwen3.5:9b				## Size:6.6GB, Context:256k, Input:text,image
ollama pull qwen3.5:27b				## Size:17GB, Context:256k, Input:text,image
ollama pull qwen3.5:cloud			## Size:----, Context:256k, Input:text,image

ollama pull qwen3.6:27b				## Size:17GB, Context:256k, Input:text,image

ollama pull deepseek-v4-flash:cloud	## Size:----, Context:1M, Input:text

# macOS Apple Silicon
ollama pull qwen3.6:27b-coding-nvfp4	## Size:20GB, Context:256k, Input:text
## Optimal Model size for MBA 20026 (M5 chip + 16GB RAM)
#  Comfortable and Fast (recommended): 8B parameter Models with 4-bit quantization:
## llama3:8b, qwen3:8b, qwen3.5:9b
#  Maximum Potential: 13B parameter models:
## gemma3:12b, mistral:7b
```



### 7- Run local model against vibe coding agent

Rub `gemma4` model against `Claude Code` in VS Code: 

Launch VS-Code, create/select an empty folder, open Terminal and type the below to launch `OpenCode`:

```shell
ollama launch opencode --model gemma4:31b-cloud
```

Or launch `Claude Code` by:

```shell
ollama launch claude --model gemma4:31b-cloud
```

At `Security Guide`, select `1. Yes, I trust folder`.





### 8- Build Your Dream Apps

Build a few simple Games with example promptings:

Game 1:

```shell
Create a file named test.html with valid HTML5.
It should contain only a black background and a centered white heading saying Hello.
```

Game 2: 

```shell
Create a simple single-file HTML snake game using canvas API and save the file into current project folder.
```

Game 3:

```shell
Create a simple single-file HTML brick breaker game using canvas API with score and lives. Also save the file into current project folder.
```



## The End

