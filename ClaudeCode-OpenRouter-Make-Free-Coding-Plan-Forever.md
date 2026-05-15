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

Then create an API Key and note down:

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



## 3- Divert ANTHROPIC default model to OpenRouter

Go to your project folder and create a sunfolder: `.claude` , under which create a `settings.json` file:

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
        "ANTHROPIC_BASE_URL": "https://openrouter.ai/api",
        "ANTHROPIC_AUTH_TOKEN": "Your API Key",
		"ANTHROPIC_API_Key": "",
		"ANTHROPIC_MODEL": "openrouter/free"
    }
}
```

The settings above is valid for current Terminal session only.

To enable the settings permanently, add the 3 lines above into `~/.zshrc`:

```shell
## macOS
nano ~/.zshrc
## Linux
nano ~/.bashrc
```

Then add the following four lines:

```text
ANTHROPIC_BASE_URL=https://openrouter.ai/api
ANTHROPIC_AUTH_TOKEN="Your-API-Key"
ANTHROPIC_API_Key=""
ANTHROPIC_MODEL="openrouter/free"
```



## 4- Give a shot: a dinosaur game

Launch Claude:

```shell
claude
```

Please note, when writing the prompt, better to write all requirements in one shot to save tokens.

For instance, I typed in:

```text
Please use HTML_JavaScript to make Chrome-based offline dinosaur game in a single index.html file, Using space bar to jump, on a land with cactus barrier; starting from right side; It changes to Game-over if hitting a barrier, after which, score shall be displayed, plus a Restart button pops up; The display shall be in beautiful pixels.
```

I renamed the `intex.html` to `./claudeDemo/dino-game-gemma4.html`, it's not playable, to be honest.



## The End















