## Claude Code + OpenRouter Make Coding Forever Free

Marcus Zou | 14 May 2026



## Intro

Token fee is soaring to sky.



## 1- Install Claude Code

```shell
# macOS/Linux: Bash install it
curl -fsSL https://claude.ai/install.sh | bash
## npm nstall -g @anthropic-ai/claude
## macOS Only
## brew install --cask claude-code@latest

# Add the env
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc
# Check version
claude -v
```



## 2- Register an OpenRouter account and Create API Key

Go to https://openrouter.ai to register a free account;

Then create an API Key and note it down.



## 3- Divert ANTHROPIC default models to OpenRouter

### 3A. For project only

Go to your project folder and create a subfolder: `.claude` , under which create a `settings.json` file:

```shell
cd projects/claudeDemo
mkdir -p .claude
cd .claude
nano settings.json
```

Ensure the following contents are in place:

```json
{
	"env": {
        "ANTHROPIC_AUTH_TOKEN": "Your-API-Key-created-from-openrouter",        			         "ANTHROPIC_BASE_URL": "https://openrouter.ai/api",
		"ANTHROPIC_API_Key": "",
		"ANTHROPIC_MODEL": "openrouter/free"
    }
}
```

The settings above is valid for current Terminal session only.

### 3B. For all Claude events

**To enable the settings permanently**, add the lines above into `~/.zshrc` (**macOS**) or `~/.bashrc` (**Linux**):

```shell
## macOS
nano ~/.zshrc
## Linux
nano ~/.bashrc
```

Then add the following four lines:

```text
export ANTHROPIC_BASE_URL="https://openrouter.ai/api"
export ANTHROPIC_AUTH_TOKEN="Your-API-Key-created-from-openrouter"
export ANTHROPIC_API_Key=""
export ANTHROPIC_MODEL="openrouter/free"
```

or edit `~\.claude\settings.json` (**Windows PowerShell**):

```shell
## PowerShell in Windows
nano ~\.claude\settings.json
```

Then add the following four lines:

```json
{
	"env": {
        "ANTHROPIC_AUTH_TOKEN": "Your-API-Key-created-from-openrouter",        			         "ANTHROPIC_BASE_URL": "https://openrouter.ai/api",
		"ANTHROPIC_API_Key": "",
		"ANTHROPIC_MODEL": "openrouter/free"
    }
}
```



Please note: by specifying "**openrouter/free**" as ANTHROPIC_MODEL, the **OpenRouter** system will rotate the [free models](https://openrouter.ai/openrouter/free) when you request the LLM models via this settings. Currently the free models are:

```shell
- gpt-oss-120b	4.24B by OpenAI
- gpt-oss-20b	2.56B by OpenAI
- MiniMax M2.5	2.13B by mimnimax
- Nemotron 3 Nano 30B A3B	1.97B by NVIDIA
- Trinity Large Thinking	1.81B by Arcee-AI
```



Sometimes, you may need a specific model to serve your specific purpose, then please tune the settings as below:

```text
export ANTHROPIC_AUTH_TOKEN="Your-API-Key-created-from-openrouter"
export ANTHROPIC_BASE_URL="https://openrouter.ai/api"
export ANTHROPIC_DEFAULT_HAIKU_MODEL="deepseek/deepseek-v4-flash:free"
export ANTHROPIC_DEFAULT_OPUS_MODEL="deepseek/deepseek-v4-flash:free"
export ANTHROPIC_DEFAULT_SONNET_MODEL="deepseek/deepseek-v4-flash:free"
```

back to the project setting file: `~/projects/claudeDemo/.claude/settings.json`, which belike:

```json
{
	"env": {
        "ANTHROPIC_AUTH_TOKEN": "Your API Key created with OpenRouter",        			         "ANTHROPIC_BASE_URL": "https://openrouter.ai/api",
		"ANTHROPIC_DEFAULT_HAIKU_MODEL": "nvidia/nemotron-3-super-120b-a12b:free",
		"ANTHROPIC_DEFAULT_OPUS_MODEL": "nvidia/nemotron-3-super-120b-a12b:free",
		"ANTHROPIC_DEFAULT_SONNET_MODEL": "nvidia/nemotron-3-super-120b-a12b:free"
    }
}
```



For Coding, the best free models from OpenRouter.ai could be:

```text
- nvidia/nemotron-3-super-120b-a12b:free	## Free | 1M | 633B | Mar 11, 2026
- deepseek/deepseek-v4-flash:free			## Free | 1M | 62.8B | Apr 24, 2026
- qwen/qwen3-coder:free						## Free | 1M | 60.9B | Jul 23, 2025
- minimax/minimax-m2.5:free					## Free | 205K | 43.8B | Feb 12, 2026
```

For general purpose (Reasoning, MoE. MultiModal), the best FREE models from OpenRouter.ai could be:

```text
- deepseek/deepseek-v4-flash:free	## Free | 1M | 62.8B | Apr 24, 2026
- google/gemma-4-31b-it:free		## Free | 262K | 12.2B | Apr 2, 2026
- google/gemma-4-26b-a4b-it:free	## Free | 262K | 4.57B | Apr 3, 2026
```



## 4- Give a shot: a dinosaur game

Launch Claude:

```shell
claude
```

Please note, when writing the prompt, better to write all requirements in one shot to save tokens.

For instance, I typed in:

```text
Please use HTML + JavaScript to make Chrome-based offline dinosaur game in a single index.html file, Using space bar to jump, on a land with cactus barriers; starting from left side; It changes to Game-over if hitting a barrier, after which, score shall be displayed, plus a Restart button pops up; The display shall be in beautiful pixels.
```

I renamed the `intex.html` to `./claudeDemo/dino-game-gemma4.html`, it's not playable, to be honest.



## The End















