## Claude Code + Gemma4 Local Model on Mac

Marcus Zou | 12 May 2026



## Intro

Token fee is soaring to sky.



## 1- Install Claude Code

```shell
# Bash install it
curl -fsSL https://claude.ai/install.sh | bash
## Alternatively use npm
## npm nstall -g @anthropic-ai/claude

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
lms get google/gemma-4-e4b
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
export ANTHROPIC_API_Key=lmstudio
export ANTHROPIC_AUTH_TOKEN=lmstudio
```

The settings above is valid for current Terminal session only.

To enable the settings permanently, add the 2 lines above into `~/.zshrc`:

```shell
nano ~/.zshrc
```



## 6- Launch the local model the first time

```shell
claude --model google/gemma-4-e4b
```



## 7- Give a shot

A simple test run is to to build a "Hello World" app:

```shell
Please write a Hello World app using Python.
```

Then run the script:

```shell
python3 hello.py
```



## 8- Change the defult context length

```shell
# unload all models
lms unload
# Reload the model with specified context length
lms load google/gemma-4-e4b --context-length 40960

# Run Claude Code + Gemma4 model
claude --model google/gemma-4-e4b
```



## 9- Make a dino-game

Please note, when writing the prompt, better to write all requirements in one shot to save tokens.

For instance, I typed in:

```text
Please use HTML_JavaScript to make Chrome-based offline dinosour game in a single index.html file, Using space bar to jump, on a land with cactus barrier; starting from right side; It changes to Game-over if hitting a barrier, after which, score shall be displayed, plus a Restart button pops up; The display shall be in beautiful pixels.
```



## Let's Code

