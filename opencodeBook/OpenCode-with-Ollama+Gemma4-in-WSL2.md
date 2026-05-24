# Blow Your Mind: OpenCode with Ollama+Gemma4 in WSL2

 

## Mindset

- Have to work in Windows env daily, and
- Familiar and comfortable with WSL2 and Linux, and
- Love Vibe Coding within Linux, and
- Don't wanna go dual-booting the workstation, and
- Wanna cut cost of tokens



## Approaches

- Install WSL2 Ubuntu24
- Install `ollama` - taking care of local LLM management
- Install `llmfit` to filter the model my workstation can handle
- Install `OpenCode` to manage Vibe coding process



## Operations



1- Enable `Windows Subsystems for Linux` and Install WSL2 distro, say Ubuntu24.04

```shell
# Check out what WSL instance can be installed
wsl -l -o
# Install specific Linux distro
wsl --install Ubuntu-24.04
```

Some pre-jobs:

```shell
# Pre-jobs
apt update
apt install nano wget git tar zip unzip tree ntfs-3g -y
apt install zstd
apt autoremove
```



2- Install `Ollama` in WSL2 Ubuntu24

```shell
# Install ollama
curl -fsSL https://ollama.com/install.sh | sh
####
#### Nvidia GPU detected
#### The Ollama API is now available at 127.0.0.1:11434
####

# Check the version
ollama -v
```



3- Install `llmfit`  (optional)

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
# Google family
ollama pull gemma4:e4b
ollama pull gemma4:26b
ollama pull gemma4:31b-cloud
# Qwen Family
ollama pull qwen3:14b
ollama pull qwen3-coder:30b
ollama pull qwen3-coder:480b-cloud
ollama pull qwen3-coder-next:cloud
```

Then let's give a shot, by the way - the format on `ollama` site slightly differ from `llmfit`'s output

```text
ollama run gemma4:e4b
ollama run qwen3:14b
```



4- Install `OpenCode` in WSL2 Ubuntu24

```shell
# Install opencode
curl -fsSL https://opencode.ai/install | bash
# check the version
opencode -v
# Give a shot
opencode
```



5- Run local LLM model `gemma4` coupled with `OpenCode`

```shell
ollama launch opencode --model gemma4:31b-cloud
```



## The End

