# Claude Code + LM Studio + Gemma4 Local Model on Mac/Linux

Marcus Zou | 12 May 2026



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



## 2- Install LM Studio

```shell
# Use bash
curl -fsSL https://lmstudio.ai/install.sh | bash
# to start the daemon, run
lms daemon up
# open a new terminal and check the version
lms -v
```



## 3- Pull down Gemma4 model

```shell
## Smaller model in macOS (MacBook Air M5 - 16GB RAM)
lms get google/gemma-4-e4b
## Bigger model on Dell Precision 7820 Tower - 256GB RAM + 16G VRAM x 2
lms get google/gemma-4-31b
lms get google/gemma-4-26b-a4b
```



## 4- Start LM Studio Server

```shell
# Start the Server
lms server start --port 1234
## Success! Server is now running on port 1234

# Check the status
lms status
## Server: ON (port: 1234) ... No Models Loaded
```



## 5- Divert ANTHROPIC default path to Local Model

```shell
export ANTHROPIC_BASE_URL=http://localhost:1234
export ANTHROPIC_AUTH_TOKEN=lmstudio
# export ANTHROPIC_API_Key=lmstudio
```

The settings above is valid for current Terminal session only.

To enable the settings permanently, add the 3 lines above into `~/.zshrc`:

```shell
## macOS
nano ~/.zshrc
## Linux
nano ~/.bashrc
```



## 6- Launch the local model the first time

```shell
## create a folder
mkdir -p ~/projects/claudeDemo
cd ~/projects/claudeDemo
## macOS case
claude --model google/gemma-4-e4b
## load the monster model into the beast machine: Dell Precision 7820
claude --model google/gemma-4-31b
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



## 8- Give a shot

A simple test run is to to build a "Hello World" app:

```shell
Please write a Hello World app using Python and save to current folder and run it up.
```

Then run the script:

```shell
python3 hello.py
```

**In case of error: libgomp.so.1: cannot open shared object file: No such file or directory**

```shell
## Debian/Ubuntu
sudo apt-get update
sudo apt-get install libgomp1
## RHEL/Fedora/Rocky
sudo dnf update
sudo dnf install libgomp
```



## 9- Even more fun: a dinosaur game

Please note, when writing the prompt, better to write all requirements in one shot to save tokens.

For instance, I typed in:

```text
Please use HTML + JavaScript to make Chrome-based offline dinosaur game in a single index.html file, Using space bar to jump, on a land with cactus barriers; starting from left side; It changes to Game-over if hitting a barrier, after which, score shall be displayed, plus a Restart button pops up; The display shall be in beautiful pixels.
```

I renamed the `intex.html` to `./claudeDemo/dino-game-gemma4.html`, it's not playable, to be honest.



## Codex GPT 5.5 Knocks Out Gemma4 when vibe-coding

I downloaded Codex using GPT 5.5 to build the same dinosaur game and it turns out this version dino game is totally playable. Then GPT 5.5 KO Gemma4 in terms of vibe coding domain.

Here is the `./claudeDemo/dino-game-codex-gpt55.html`, go take a look.

```shell
## macOS
open ./claudeDemo/dino-game-codex-gpt55.html
## Linux
## apt install xdg-utils
xdg-open ./claudeDemo/dino-game-codex-gpt55.html
```





## The End
