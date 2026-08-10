# Codex CLI and App: Getting Started

 

[TOC]

## Part A - Operations - Official



### 1- Pre-requisites

Install Git, Node.JS, Python3, GitHub.Cli (optional) and DotNet SDK (Windows only).

```shell
# Windows
# -----------------
winget install --id Git.Git
winget install --id OpenJS.NodeJS.LTS
winget install --id Python.Python.3.12 # 3.14 is also good, but 3.12 for better compatibility
winget install --id Microsoft.DotNet.SDK.10
winget install --id GitHub.cli

# macOS
# -----------------
brew update
brew install git gh python@3.12 node@24

# Debian/Ubuntu
# -----------------
## Install Git
apt install git
## Install GitHub.Cli (gh)
wget https://github.com/cli/cli/releases/download/v2.97.0/gh_2.97.0_linux_amd64.deb
sudo dpkg -i gh_2.97.0_linux_amd64.deb

## Download and setup the NodeSource LTS repository and install NodeJS
curl -fsSL https://nodesource.com | sudo -E bash -
sudo apt-get install -y nodejs

# Fedora/RHEL/Rocky
# -----------------
## Install Git
dnf install git
## Install GitHub.Cli (gh)
dnf install https://github.com/cli/cli/releases/download/v2.97.0/gh_2.97.0_linux_amd64.rpm

## Install NodeJS 24 (LTS)
curl -fsSL https://rpm.nodesource.com/setup_24.x | sudo bash -
dnf install -y nodejs
```



### 2.1- Install Codex CLI (the TUI version)

Install `Codex CLI`  as vibe coding agent.

```shell
# Mac/Linux
curl -fsSL https://chatgpt.com/codex/install.sh | sh
## or via npm
npm install -g @openai/codex
## or via Homebrew
brew install --cask codex

# Windows
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
## Or via MS Store (may have issue - cannot find codex)
winget install Codex -s msstore
winget install OpenAi.Codex
## Or via npm
npm install -g @openai/codex
```

Codex CLI: Version check and upgrade:

```shell
# Version check
codex --version

# Upgrade Codex CLI
npm i -g @openai/codex@latest
```



### 2.2- Install Codex App (the GUI version)

Download the [Codex app](https://get.microsoft.com/installer/download/9PLM9XGG6VKS?cid=website_cta_psi) for Windows.

Download the [Codex App](https://persistent.oaistatic.com/codex-app-prod/Codex.dmg) for macOS.

Download the community-based [Codex-App](https://github.com/cuongducle/codex-linux/releases/download/v26.803.41515/codex-desktop-26.803.41515-linux-amd64.deb) for Linux, or run the one-liner:

```shell
curl -fsSL https://cuongducle.github.io/codex-linux/install.sh | sudo bash
```



### 3.1- Run Codex CLI

Simply run `codex` to get started. Then sign in as prompted.

```shell
codex
```

or Alternatively, you can authenticate using an **API key** by setting an environment variable:

```shell
export OPENAI_API_KEY="your-api-key-here"
codex
```

### 3.2- Run Codex App

Launch Codex App from your Applications folder (mac) or Start Menu (Windows).



## Part B - Operations with Local Models



### 0- Which model my computer can run smoothly?

A popular to tell which models can be run on my computers is `llmfit`. Install it as below:

```shell
# Windows
## Pre-install scoop PkgManager
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
## Install llmfit
scoop install llmfit

# macOS/Linux
curl -fsSL https://llmfit.axjns.dev/install.sh | sh
curl -fsSL https://llmfit.axjns.dev/install.sh | sh -s -- --local

# Usage - TUI (default)
llmfit
```



### 1- Install & Launch `ollama` model

```shell
# Install ollama
## Windows
irm https://ollama.com/install.ps1 | iex
## macOS/Linux
sudo apt install zstd
curl -fsSL https://ollama.com/install.sh | sh

# List models installed
ollama ls

# pull & serve
ollama pull qwen3.6:27b     ## Qwen3.6:27b is better then Qwen3.5-coder per se
ollama serve
```



### 2- Connect via Codex App or CLI

**Codex App:** Simply run `ollama launch codex-app` in your terminal, and select your local model.

**Codex CLI:** As per documents of `OpenAI` company -

Add the `Ollama` profile to your `~/.codex/config.toml` file:

```toml
[model_providers.ollama_custom]
name = "Ollama Local"
base_url = "http://localhost:11434/v1"
wire_api = "responses"
```

Also edit the `~/.codex/local.config.toml` file:

```toml 
[profiles.local]
model_provider = "ollama"
model = "qwen3.6:27b"
```

Then run Codex using: 

```shell
codex --profile local
```



Once you launch up the `TUI` or `GUI` version of Codex, it may still state `GPT-5.5` even though your local models are answering the queries, don't worry - **this is purely a visual cosmetic bug and your local `Ollama` model is actually running under the hood****.** [[1](https://www.youtube.com/watch?v=qo40REk9wNU)]

**How to Force the TUI to Show the Right Name**

If the misleading visual label bothers you, you can explicitly force Codex to refresh its model tag by manually specifying your target local model on launch using the `--model` flag:

```bash
ollama launch codex-app --model qwen3.6:27b
```

*(Swap `qwen3.6:27b` out for whatever specific model you have pulled in Ollama, such as `llama3`, `gemma4`, etc.)*. 

This pushes the correct local string directly into Codex's runtime session variables, which usually fixes the TUI header text.

### 3- A quick run of Codex CLI  against local model qwen3.6:27b

```shell 
ollama launch codex --model qwen3.6:27b
```

Then give out a dedicated prompt:

```text
Please design and implement a web app called "DiaBit" for Directional Drilling Survey calculation for Oil and gas Industry using "minimum curvature algorithm".
- The typical product to mimick is Halliburton/Landmark's EDT or Compass, or Paradigm's SysDril.
- Using React/Next.JS and Tailwind CSS as frontend language (advise if you have better options), a database of sqlite3 is needed to manage the survey data: importing, saving, processing, and exporting. 
- An admin panel shall be in place for manage datasets and user profiles. 
- The frontend interface shall be sleek, with a header, a body and a footer.
  -- The header bar: Help, About buttons at left corner, leading to 2 seperate pages; Logo and app name "DiaBit" and its version in the center, user sign-in and light/dark toggle icons at right corner
  -- The body part: a sidebar at left side, occupying 20% width, autohide if idle 30 seconds, with contents of hirarchy of (Country - State/Province - geoBasin - Field - Well - Slot - trajectory plans and deviation surveys) in tree-style design, clicking a specific "Trajectory Plan" or "Deviation Survey" shall bring up the table into the workspace in the center (occupy 60% width), with the table being displayed in grids in Excel format/layout, once inserting or appending a line of MD/Inclination/Azimuth in the table, the other parameters (X, Y, Z, TVD, Easting, Northing, dogleg Severity, etc.) shall calculated automatically. A Calculation Settings sidebar shall be presented at right, occupying 20% width of the page, also autohide if idle 30 seconds. 
  -- The body part (Cont'ed): under the data table mentioned above, there shall be a 3D plot of the trajectory (planned vs. real-time), which sits above 2 side-by-side plots of the Plan view, and the vertical view.
  -- The footer bar: Disclaimer button at left corner which connects to a pop-up dialogue; "@2026 All rights reserved by Alfazen Inc." in the center, and social icons: web, twitter/X, linkedin at right corner).
- Version Control:
  -- the version number shall be in the format of "m.n.p" starting from 1.0.0, while "n" and "p" are only 1-digit, incrementign from 0 to 9.
  -- there shall be pre-commit hook to bump the version by 0.0.1 for every build.
  -- when git commiting, the message shall be "vm.n.p Build 2026-06-26-hh-mm" + the feat message.
- If venv is needed, please use uv.
- Go through tests till no issues is found.
- All priviledges needed in this project folder are granted, no need to ask permissions.
- the database shall be sub-folder of ./data/
- Critical Designs, implementation plans and workarounds shall be recoded into a tech-notes.md file in subfolder of ./docs/ and keep updated till project ends.
- Run the app at Port 3020, or 3030 if 3020 is not available.
- Prepare AGENTS.md, README.md (Including at least a intro of the DiaBit app and teck stack for this dev) and Dockerfile, docker-compose.yml
- Eventually report how many tokens have been costed and how many minutes have taken into the last part of the Tech-notes.
```



## Part C - Operations: The Qwable Model

> [!NOTE]
>
> https://huggingface.co/lordx64/Qwable-v1
>
> **Qwen + Fable** An open-weights agentic coding model. 35B Mixture-of-Experts (3B active), built by layering Claude Fable-5 agentic tool-use behavior on top of a Claude Opus 4.7 reasoning distill of Qwen3.6-35B-A3B.

**TL;DR**

Qwable-v1 is a **chained distill**: vanilla Qwen3.6-35B-A3B → SFT on Claude Opus 4.7 reasoning traces → SFT on Claude Fable-5 agentic tool-use traces. The result is an open-weights model that:

- **Thinks** in explicit `<think>…</think>` chains-of-thought (inherited from the Opus 4.7 prior)
- **Acts** like a Claude-Code-style agent when prompted as one — emits `<tool_use>` XML blocks for file edits, shell commands, and reads (added by the Fable-5 SFT). The XML format is **system-prompt-conditional**: it appears when you give the model an agent-style system prompt or supply a preceding `<tool_result>` turn. With a bare prompt and no agent framing, the model falls back to the Opus 4.7 reasoning-and-explain prior. See [Usage](https://huggingface.co/lordx64/Qwable-v1#usage) for the recipe.
- Runs on a single H200 / 2× A100-80GB at bf16, or any 24+ GB consumer GPU at IQ4_XS quantization

GGUF quants at [`lordx64/Qwable-v1-GGUF`](https://huggingface.co/lordx64/Qwable-v1-GGUF):

- **IQ4_XS** (~18 GB) — runs on 24 GB consumer GPUs (3090, 4090), LM Studio default
- **Q4_K_M** (~21 GB) — better quality, fits 32-48 GB workstations
- **Q5_K_M** (~25 GB) — better quality, fits 32-48 GB workstations
- **Q8_0** (~37 GB) — near-lossless, for reproducibility checks

The pull the model down:

```shell
ollama pull hf.co/lordx64/Qwable-v1-GGUF:IQ4_XS
```



## Part D - Integrating with Codex App

Just in case you have installed Codex CLI in WSL, plus a Codex App on Windows, and you still need to use Codex CLI,

1A) Open Codex App, and go to "Settings" > "Settings" > "General" > Change "Agent Environment" to "Windows Subsystem for Linux"; and change "Integrated terminal shell" to "WSL".

1B) Install `bubblewrap` in WSL to eliminate the complains: **Codex could not find `bubblewrap` on PATH**.

```shell
sudo apt update && sudo apt install -y bubblewrap
bwrap --version
```

1C) In case of error: **MCP client for 'codex_apps' failed to start**, please run:

```shell
# Completely clear out the cached configurations and state
rm -rf ~/.codex ~/.cache/codex

# Kill any orphaned Codex backend or node processes
pkill -f codex

# Clear common proxy environments for the session
unset HTTP_PROXY HTTPS_PROXY http_proxy https_proxy ALL_PROXY all_proxy

# Update Codex CLI globally to the newest version
npm update -g @openai/codex

# Start Codex clean
codex
```



## Part E - OpenAI authentication

Codex supports two ways to sign in when using OpenAI models:

- `Sign in with ChatGPT` for subscription access
- `Sign in with an API key` for usage-based access

Codex cloud requires signing in with ChatGPT. The Codex CLI and IDE extension support both sign-in methods.

Your sign-in method also determines which admin controls and data-handling policies apply.

- With `sign in with ChatGPT`, Codex usage follows your ChatGPT workspace permissions, RBAC, and ChatGPT Enterprise retention and residency settings
- With `sign in with an API key`, usage follows your API organization’s retention and data-sharing settings instead

For the CLI, Sign in with ChatGPT is the default authentication path when no valid session is available.



### 1- Sign in with ChatGPT

When you sign in with ChatGPT from the Codex app, CLI, or IDE Extension, Codex opens a browser window for you to complete the login flow. After you sign in, the browser returns an access token to the CLI or IDE extension.

If your environment already provides a ChatGPT access token, the CLI can read it from stdin:

```
printenv CODEX_ACCESS_TOKEN | codex login --with-access-token
```

#### ChatGPT Pricing (personal)

| Free Tier                                                    | Go                                                           | Plus                                                         | Pro                                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| $0/month                                                     | $5.50/month                                                  | $20/month                                                    | $100/month                                                   |
|                                                              | Everything in Free and:                                      | Everything in Go and:                                        | Everything in Plus and:                                      |
| * Limited access to GPT-5.5 Instant<br />* Limited messages and uploads  <br />* Limited and slower image generation  <br />* Limited deep research  <br />* Limited memory and context  <br />* Limited Codex access | * More access to GPT-5.5 Instant  <br />* More messages<br />* More uploads  <br />* More image creation  <br />* Longer memory | * Advanced reasoning with GPT-5.5 Thinking <br />* Expanded messages and uploads  <br />* More complex and accurate image creation  <br />* Expanded deep research and agent mode  <br />* Expanded memory and context  <br />* Projects, tasks, and custom GPTs  <br />* Expanded Codex usage  <br />* Early access to new features | * 5x or 20x more usage  <br />* 5x 10x or 20x more Codex usage  <br />* Pro reasoning with GPT-5.5 Pro  <br />* Maximum Codex tasks  <br />* Unlimited GPT-5.3 and file uploads  <br />* Unlimited and faster image creation  <br />* Maximum deep research and agent mode  <br />* Maximum memory and context  <br />* Expanded projects, tasks, and custom GPTs  <br />* Research preview of new features |



### 2- Sign in with an API key

You can also sign in to the Codex app, CLI, or IDE Extension with an API key. Get your API key from the OpenAI dashboard: https://platform.openai.com/api-keys.

#### Codex API Pricing

OpenAI bills API key usage through your OpenAI Platform account at standard API rates. See the [API pricing page](https://openai.com/api/pricing/).

| GPT-5.5                                                      | GPT-5.4                                                   | GPT-5.4 Mini                                                 |
| ------------------------------------------------------------ | --------------------------------------------------------- | ------------------------------------------------------------ |
| A new class of intelligence for coding and professional work. | A more affordable model for coding and professional work. | Our strongest mini model yet for coding, computer use, and subagents. |
| Price                                                        | Price                                                     | Price                                                        |
| Input: $5.00 / 1M tokens                                     | Input: $2.50 / 1M tokens                                  | Input: $0.75 / 1M tokens                                     |
| Cached input: $0.50 / 1M tokens                              | Cached input: $0.25 / 1M tokens                           | Cached input: $0.075 / 1M tokens                             |
| Output: $30.00 / 1M tokens                                   | Output: $15.00 / 1M tokens                                | Output: $4.50 / 1M tokens                                    |

Features that rely on ChatGPT credits, such as [fast mode](https://developers.openai.com/codex/speed), are available only when you sign in with ChatGPT. If you sign in with an API key, Codex uses standard API pricing instead.

We recommend API key authentication for programmatic Codex CLI workflows, such as CI/CD jobs. Don’t expose Codex execution in untrusted or public environments.



## Part F - Secure your Codex cloud account

Codex cloud interacts directly with your codebase, so it needs stronger security than many other ChatGPT features. Enable multi-factor authentication (MFA).

If you use a social login provider (Google, Microsoft, Apple), you aren’t required to enable MFA on your ChatGPT account, but you can set it up with your social login provider.



### 1- Login caching

When you sign in to the Codex app, CLI, or IDE Extension using either ChatGPT or an API key, Codex caches your login details and reuses them the next time you start the CLI or extension. The CLI and extension share the same cached login details. If you log out from either one, you’ll need to sign in again the next time you start the CLI or extension.

Codex caches login details locally in a plaintext file at `~/.codex/auth.json` or in your OS-specific credential store.

For sign in with ChatGPT sessions, Codex refreshes tokens automatically during use before they expire, so active sessions usually continue without requiring another browser login.

### 2- Credential storage

Use `cli_auth_credentials_store` to control where the Codex CLI stores cached credentials:

```
# file | keyring | auto
cli_auth_credentials_store = "keyring"
```

- `file` stores credentials in `auth.json` under `CODEX_HOME` (defaults to `~/.codex`).
- `keyring` stores credentials in your operating system credential store.
- `auto` uses the OS credential store when available, otherwise falls back to `auth.json`.

If you use file-based storage, treat `~/.codex/auth.json` like a password: it contains access tokens. Don’t commit it, paste it into tickets, or share it in chat.

### 3- Enforce a login method or workspace

In managed environments, admins may restrict how users are allowed to authenticate:

```
# Only allow ChatGPT login or only allow API key login.
forced_login_method = "chatgpt" # or "api"

# When using ChatGPT login, restrict users to a specific workspace.
forced_chatgpt_workspace_id = "00000000-0000-0000-0000-000000000000"
```

If the active credentials don’t match the configured restrictions, Codex logs the user out and exits.

These settings are commonly applied via managed configuration rather than per-user setup. See [Managed configuration](https://developers.openai.com/codex/enterprise/managed-configuration).

### 4- Login diagnostics

Direct `codex login` runs write a dedicated `codex-login.log` file under your configured log directory. Use it when you need to debug browser-login or device-code failures, or when support asks for login-specific logs.



## Part G - Build Your Dream Apps

Build a few simple Games with example promptings:

Game 1:

```shell
Tell me about this project
```

then,

```shell
Build a classic Snake game using canvas API in this repo
```

Game 2:

```shell
Create a simple single-file HTML brick breaker game using canvas API with score and lives. Also save the file into current project folder.
```



## The End

