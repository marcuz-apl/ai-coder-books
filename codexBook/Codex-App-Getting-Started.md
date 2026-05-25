# Codex with 3rd-Party Models Makes Token Free

 

[TOC]

## Mindset

- Have to work in Windows env and WSL2 daily, and
- Love Vibe Coding with CLI, and
- Wanna cut cost of tokens to some low level



## Approaches

- Install `Codex CLI` to manage Vibe coding process
- Configure `DeepSeek v4 Flash/Pro` LLM Models to make cheaper coding plan



## Operations



### 1- Install Git

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



### 2- Install Codex CLI

Install `Codex CLI`  as vibe coding agent.

```shell
# Run the following on Mac or Linux to install Codex CLI:
curl -fsSL https://chatgpt.com/codex/install.sh | sh

# Run the following on Windows to install Codex CLI:
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
```

Codex CLI can also be installed via the following package managers:

```shell
# Install using npm
npm install -g @openai/codex

# Install using Homebrew in macOS/Linux
brew install --cask codex

# Version check
codex --version
## codex-cli 0.133.0

# Upgrade Codex CLI
npm i -g @openai/codex@latest
```



### 3- Run Codex CLI

Simply run `codex` to get started. Then sign in as prompted.

```shell
codex
```

or Alternatively, you can authenticate using an **API key** by setting an environment variable:

```shell
export OPENAI_API_KEY="your-api-key-here"
codex
```



## OpenAI authentication

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

### 2- Sign in with an API key

You can also sign in to the Codex app, CLI, or IDE Extension with an API key. Get your API key from the OpenAI dashboard: https://platform.openai.com/api-keys.



## Codex API Pricing

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



## Secure your Codex cloud account

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



## Build Your Dream Apps

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

