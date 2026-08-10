# OpenCode Operations Guide

 

[TOC]

## Mindset

- Preferably work in Linux Environment
- Love Vibe Coding, and
- Able to cut cost of tokens, and
- Wanna know the OpenCode operations from A to Z



## Assumptions

- Have Linux installed, WSL2 Ubuntu24 also works well
- Have installed `ollama`, taking care of local LLM management
- Have pulled some LLM models, and running them up
- Have installed `OpenCode` to manage Vibe coding process



## Operations

The reference is actually the [OpenCode Docs site](https://opencode.ai/docs). This is where we can find the Bible of `OpenCode`.



### 1- Install `OpenCode` CLI or Desktop

Go to https://opencode.ai/download or type in:

```shell
# OpenCode CLI (Terminal)
curl -fsSL https://opencode.ai/install | bash
### Or
npm i -g opencode-ai
### or in macOS
brew install anomalyco/tap/opencode

# OpenCode Desktop (Beta)
### macOS
brew install --cask opencode-desktop
### or Download the DMG
wget https://opencode.ai/download/stable/darwin-aarch64-dmg/opencode-desktop.dmg
### Windows
wget "https://opencode.ai/download/stable/windows-x64-nsis/opencode desktop installer.exe"
### Linux Debian/Ubuntu
wget https://opencode.ai/download/stable/linux-x64-deb/opencode-desktop-linux-amd64.deb
### Linux RHEL/Rocky/Fedora
wget https://opencode.ai/download/stable/linux-x64-rpm/opencode-desktop-linux-x64_64.rpm
```



### 2- Configure `config.json` file for hosting local `ollama` models

Give a shot:

```shell
nano ~/.config/opencode/opencode.json
```

The contents belike:

```shell
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "ollama": {
      "npm": "@api-sdk/openai-compatible",
      "name": "Ollama (local)",
      "options": {
        "baseURL": "http://localhost:11434/v1"
      },
      "models": {
        "qwen3.6:27b": {
            "name": "Qwen3.6:27b"
          }
      }
    }
  }
}
```

Then, go to Project folder and run:

```shell
## Run:
opencode
```

The default model shall be loaded, but feel free to change the model to your default one:

```shell
/models
```

there are quite a few FREE models from `OpenCode Zen`: 

```text
- Big Pickle
- Nemotron 3 Super Free
- DeepSeek V4 Flash Free
```

but change the model to "**qwen3-coder-next:cloud**".



### 3- Providers

Let's play it in a hard way:



### 4- Network



### 5- Windows



### 6- Configure Tools



This is where to manage the tools an LLM can use. Tools allow the LLM to perform actions in your codebase. OpenCode comes with a set of built-in tools, but you can extend it with [custom tools](https://opencode.ai/docs/custom-tools) or [MCP servers](https://opencode.ai/docs/mcp-servers).

By default, all tools are **enabled** and don’t need permission to run. You can control tool behavior through [permissions](https://opencode.ai/docs/permissions).

[Configure](https://opencode.ai/docs/tools/#configure)

Use the `permission` field to control tool behavior. You can allow, deny, or require approval for each tool.

```
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "edit": "deny",
    "bash": "ask",
    "webfetch": "allow"
  }
}
```

You can also use wildcards to control multiple tools at once. For example, to require approval for all tools from an MCP server:

```
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "mymcp_*": "ask"
  }
}
```

[Learn more](https://opencode.ai/docs/permissions) about configuring permissions.

------

[Built-in](https://opencode.ai/docs/tools/#built-in)

Here are all the built-in tools available in OpenCode.

------

[bash](https://opencode.ai/docs/tools/#bash)

Execute shell commands in your project environment.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "bash": "allow"
  }
}
```

This tool allows the LLM to run terminal commands like `npm install`, `git status`, or any other shell command.

------

[edit](https://opencode.ai/docs/tools/#edit)

Modify existing files using exact string replacements.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "edit": "allow"
  }
}
```

This tool performs precise edits to files by replacing exact text matches. It’s the primary way the LLM modifies code.

------

[write](https://opencode.ai/docs/tools/#write)

Create new files or overwrite existing ones.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "edit": "allow"
  }
}
```

Use this to allow the LLM to create new files. It will overwrite existing files if they already exist.

Note

The `write` tool is controlled by the `edit` permission, which covers all file modifications (`edit`, `write`, `apply_patch`).

------

[read](https://opencode.ai/docs/tools/#read)

Read file contents from your codebase.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "read": "allow"
  }
}
```

This tool reads files and returns their contents. It supports reading specific line ranges for large files.

------

[grep](https://opencode.ai/docs/tools/#grep)

Search file contents using regular expressions.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "grep": "allow"
  }
}
```

Fast content search across your codebase. Supports full regex syntax and file pattern filtering.

------

[glob](https://opencode.ai/docs/tools/#glob)

Find files by pattern matching.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "glob": "allow"
  }
}
```

Search for files using glob patterns like `**/*.js` or `src/**/*.ts`. Returns matching file paths sorted by modification time.

### 7- Configure Rules: AGENTS.md

This is where we set custom instructions for `opencode`.

You can provide custom instructions to `opencode` by creating an `AGENTS.md` file. This is similar to Cursor’s rules. It contains instructions that will be included in the LLM’s context to customize its behavior for your specific project.

------

#### [Initialize](https://opencode.ai/docs/rules/#initialize)

To create a new `AGENTS.md` file, you can run the `/init` command in opencode.

> [!TIP]
>
> You should commit your project’s `AGENTS.md` file to Git.



`/init` scans the important files in your repo, may ask a couple of targeted questions when the codebase cannot answer them, and then creates or updates `AGENTS.md` with concise project-specific guidance.

It focuses on the things future agent sessions are most likely to need:

- build, lint, and test commands
- command order and focused verification steps when they matter
- architecture and repo structure that are not obvious from filenames alone
- project-specific conventions, setup quirks, and operational gotchas
- references to existing instruction sources like Cursor or Copilot rules

If you already have an `AGENTS.md`, `/init` will improve it in place instead of blindly replacing it.

------

#### [Example](https://opencode.ai/docs/rules/#example)

You can also just create this file manually. Here’s an example of some things you can put into an `AGENTS.md` file.

AGENTS.md

```
# SST v3 Monorepo Project
This is an SST v3 monorepo with TypeScript. The project uses bun workspaces for package management.
## Project Structure
- `packages/` - Contains all workspace packages (functions, core, web, etc.)- `infra/` - Infrastructure definitions split by service (storage.ts, api.ts, web.ts)- `sst.config.ts` - Main SST configuration with dynamic imports
## Code Standards
- Use TypeScript with strict mode enabled- Shared code goes in `packages/core/` with proper exports configuration- Functions go in `packages/functions/`- Infrastructure should be split into logical files in `infra/`
## Monorepo Conventions
- Import shared modules using workspace names: `@my-app/core/example`
```

We are adding project-specific instructions here and this will be shared across your team.

------

#### [Types](https://opencode.ai/docs/rules/#types)

opencode also supports reading the `AGENTS.md` file from multiple locations. And this serves different purposes.

#### [Project](https://opencode.ai/docs/rules/#project)

Place an `AGENTS.md` in your project root for project-specific rules. These only apply when you are working in this directory or its sub-directories.

#### [Global](https://opencode.ai/docs/rules/#global)

You can also have global rules in a `~/.config/opencode/AGENTS.md` file. This gets applied across all opencode sessions.

Since this isn’t committed to Git or shared with your team, we recommend using this to specify any personal rules that the LLM should follow.

#### [Claude Code Compatibility](https://opencode.ai/docs/rules/#claude-code-compatibility)

For users migrating from Claude Code, OpenCode supports Claude Code’s file conventions as fallbacks:

- **Project rules**: `CLAUDE.md` in your project directory (used if no `AGENTS.md` exists)
- **Global rules**: `~/.claude/CLAUDE.md` (used if no `~/.config/opencode/AGENTS.md` exists)
- **Skills**: `~/.claude/skills/` — see [Agent Skills](https://opencode.ai/docs/skills/) for details

To disable Claude Code compatibility, set one of these environment variables:

Terminal window

```
export OPENCODE_DISABLE_CLAUDE_CODE=1        # Disable all .claude support
export OPENCODE_DISABLE_CLAUDE_CODE_PROMPT=1 # Disable only ~/.claude/CLAUDE.md
export OPENCODE_DISABLE_CLAUDE_CODE_SKILLS=1 # Disable only .claude/skills
```

------

#### [Precedence](https://opencode.ai/docs/rules/#precedence)

When opencode starts, it looks for rule files in this order:

1. **Local files** by traversing up from the current directory (`AGENTS.md`, `CLAUDE.md`)
2. **Global file** at `~/.config/opencode/AGENTS.md`
3. **Claude Code file** at `~/.claude/CLAUDE.md` (unless disabled)

The first matching file wins in each category. For example, if you have both `AGENTS.md` and `CLAUDE.md`, only `AGENTS.md` is used. Similarly, `~/.config/opencode/AGENTS.md` takes precedence over `~/.claude/CLAUDE.md`.

------

#### [Custom Instructions](https://opencode.ai/docs/rules/#custom-instructions)

You can specify custom instruction files in your `opencode.json` or the global `~/.config/opencode/opencode.json`. This allows you and your team to reuse existing rules rather than having to duplicate them to AGENTS.md.

Example:

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["CONTRIBUTING.md", "docs/guidelines.md", ".cursor/rules/*.md"]
}
```

You can also use remote URLs to load instructions from the web.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["https://raw.githubusercontent.com/my-org/shared-rules/main/style.md"]
}
```

Remote instructions are fetched with a 5 second timeout.

All instruction files are combined with your `AGENTS.md` files.

------

#### [Referencing External Files](https://opencode.ai/docs/rules/#referencing-external-files)

While opencode doesn’t automatically parse file references in `AGENTS.md`, you can achieve similar functionality in two ways:

#### [Using opencode.json](https://opencode.ai/docs/rules/#using-opencodejson)

The recommended approach is to use the `instructions` field in `opencode.json`:

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["docs/development-standards.md", "test/testing-guidelines.md", "packages/*/AGENTS.md"]
}
```

#### [Manual Instructions in AGENTS.md](https://opencode.ai/docs/rules/#manual-instructions-in-agentsmd)

You can teach opencode to read external files by providing explicit instructions in your `AGENTS.md`. Here’s a practical example:

AGENTS.md

```
# TypeScript Project Rules
## External File Loading
CRITICAL: When you encounter a file reference (e.g., @rules/general.md), use your Read tool to load it on a need-to-know basis. They're relevant to the SPECIFIC task at hand.
Instructions:
- Do NOT preemptively load all references - use lazy loading based on actual need- When loaded, treat content as mandatory instructions that override defaults- Follow references recursively when needed
## Development Guidelines
For TypeScript code style and best practices: @docs/typescript-guidelines.mdFor React component architecture and hooks patterns: @docs/react-patterns.mdFor REST API design and error handling: @docs/api-standards.mdFor testing strategies and coverage requirements: @test/testing-guidelines.md
## General Guidelines
Read the following file immediately as it's relevant to all workflows: @rules/general-guidelines.md.
```

This approach allows you to:

- Create modular, reusable rule files
- Share rules across projects via symlinks or git submodules
- Keep AGENTS.md concise while referencing detailed guidelines
- Ensure opencode loads files only when needed for the specific task



### 7- Usage





## The End

