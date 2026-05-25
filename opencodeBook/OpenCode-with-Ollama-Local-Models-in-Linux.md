# OpenCode with Ollama Local Models in Linux

 

[TOC]

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



1- Preferably install WSL2 distro, say Ubuntu24.04

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
ollama pull gemma4:e4b				## Size:9.6GB, Context:128k, Input:text,image
ollama pull gemma4:26b				## Size:17GB, Context:256k, Input:text,image
ollama pull gemma4:31b				## Size:20GB, Context:256k, Input:text,image
ollama pull gemma4:31b-cloud		## Size:----, Context:256k, Input:text,image
# Qwen Family
ollama pull qwen3:14b				## Size:13GB, Context:256k, Input:text
ollama pull qwen3-coder:30b			## Size:19GB, Context:256k, Input:text
ollama pull qwen3-coder:480b-cloud	## Size:----, Context:256k, Input:text
ollama pull qwen3-coder-next:cloud	## Size:----, Context:256k, Input:text
# Deepseek Family
ollama pull deepseek-v4-flash:cloud	## Size:----, Context:1M, Input:text
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



6- Configure `config.json` file for hosting local models

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
        "qwen3-coder-next:cloud": {
            "name": "Qwen3-coder Next Cloud"
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



## The End

