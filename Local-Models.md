# Local Models



## Generally Great Models

__Alibaba__

```shell
# Alibaba - Qwen 3.6 for Windows/Linux
ollama run qwen3.6			## qwen3.6:35b | 256k | Text, Image | 24 GB | Default
ollama run qwen3.6:27b		## qwen3.6:27b | 256k | Text, Image | 17 GB
# Alibaba - Qwen 3.6 for macOS
ollama run qwen3.6:27b-mlx	## qwen3.6:27b-mlx | 256k | Text, Image | 20 GB
ollama run qwen3.6:35b-mlx	## qwen3.6:35b-mlx | 256k | Text, Image | 22 GB
```

__Google__

```shell
# Google - gemma4 for Windows/Linux
ollama run gemma4			## gemma4:e4b | 128k | Text, Image | 9.6 GB | Default
ollama run gemma4:12b		## gemma4:12b | 256k | Text, Image | 7.6 GB
ollama run gemma4:26b		## gemma4:26b | 256k | Text, Image | 18 GB
ollama run gemma4:31b		## gemma4:31b | 256k | Text, Image | 20 GB
# Google - gemma4 for macOS
ollama run gemma4:e2b-mlx	## gemma4:e2b-mlx | 128k | Text, Image | 6.5 GB
ollama run gemma4:e4b-mlx	## gemma4:e4b-mlx | 128k | Text, Image | 8.8 GB
ollama run gemma4:12b-mlx	## gemma4:12b-mlx | 256k | Text, Image | 7.7 GB
ollama run gemma4:26b-mlx	## gemma4:26b-mlx | 256k | Text, Image | 18 GB
ollama run gemma4:31b-mlx	## gemma4:31b-mlx | 256k | Text, Image | 19 GB

# Google - gemma4 - Cloud edition
ollama run gemma4:cloud		## gemma4:cloud | 256k | Text, Image | --- GB
ollama run gemma4:31b-cloud	## gemma4:31b-cloud | 256k | Text, Image | --- GB
```

__Nvidia__

```shell
## 24GB | 1M | text
ollama pull nemotron-3-nano:30b
```



__Connect to codex-app__

```shell
ollama launch codex-app
```

__Restore to the default Codex model__

```shell
ollama launch codex-app --restore
```

__error process in PowerShell__

```shell
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy RemoteSigned
```



## Use llama.cpp to load models

__1- modify the codex config file__

```shell
model = "Qwen3.6-27B-UD-Q5_K_XL.gguf"
model_reasoning_effort = "low"
profile = "llamacpp-codex"

model_provider = "llamacpp"

[profiles.llamacpp-codex]
model = "Qwen3.6-27B-UD-Q5_K_XL.gguf"
model_provider = "llamacpp"
model_reasoning_effort = "low"

[profiles.llamacpp-codex.windows]
sandbox = "elevated"

[model_providers.llamacpp]
name = "llama.cpp"
base_url = "http://127.0.0.1:8080/v1/"
wire_api = "responses"

[windows]
sandbox = "elevated"
```

__2- Start up the llama.cpp__

```shell
llama-server.exe ^
-m "C:\models\Qwen3.6-27B-UD-Q5_K_XL.gguf" ^
-ngl 999 ^
-c 16384 ^
-n 2048 ^
-fa on ^
--jinja ^
--host 127.0.0.1 ^
--port 8080
```




## Objectives

Record and deploy local models which can be deployed on a 16GB VRAM Computer.

```shell
# Install llmfit in Windows
## Pre-install scoop
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression
## install llmfit
scoop install llmfit

# Install llmfit in Linux/macOS
curl -fsSL https://llmfit.axjns.dev/install.sh | sh
```



## The List of the Models

- gemma4:12b - the official edition

  ```shell
  # 7.6GB | 256K | text,image
  ollama pull gemma4:12b
  ```

- qwen3.5:9b

  ```shell
  # 6.6GB | 256K | text,image
  ollama pull qwen3.5:9b
  # 17GB | 256K | text,image
  ollama pull qwen3.6:27b
  ```

  

* gemma-4-12b-agentic-fable5 by yuxinlu1

  ```shell
  # 6.87GB | 256k | text | v2 - Released on June 30, 2026 on https://huggingface.co
  ## Need recent llama.cpp to load it per se: https://huggingface.co/yuxinlu1/gemma-4-12B-agentic-fable5-composer2.5-v2-3.5x-tau2
  ## https://huggingface.co/yuxinlu1/gemma-4-12B-agentic-fable5-composer2.5-v2-3.5x-tau2-GGUF/blob/main/gemma4-v2-Q4_K_M.gguf
  ollama pull hf.co/yuxinlu1/gemma-4-12B-agentic-fable5-composer2.5-v2-3.5x-tau2:Q4_K_M
  
  # 6.87GB | 256k | text | v2 - Released on June, 2026
  ollama pull xentriom/gemma-4-12B-agentic-fable5-composer2.5-v2:Q4_K_M
  ollama launch claude --model xentriom/gemma-4-12B-agentic-fable5-composer2.5-v2
  ```

  Option A - llama.cpp (recommended): this is the `gemma4_unified` architecture — older builds won’t load it

  ```text
  @echo off
  cd /d C:\llama.cpp
  llama-server.exe ^
    -m C:\models\gemma4-v2-Q4_K_M.gguf ^
    --ctx-size 16384 ^
    --n-gpu-layers 99 ^
    --no-mmap -fa on ^
    --jinja ^
    --temp 1.0 --top-p 0.95 --top-k 64 ^
    --host 0.0.0.0 --port 18080
  pause
  ```



- qwythos-9b

  ```shell
  ## 6.8GB | 1M | text,image
  ollama pull pdurlej/qwythos-9b-claude-mythos-5-1m:Q4_K_M
  ## 6.8GB | 1M | text,image
  ollama pull hf.co/empero-ai/Qwythos-9B-Claude-Mythos-5-1M-GGUF:Q4_K_M
  ## 6.8GB | 1M | text,image
  ollama pull mikemikeok/Qwythos-9B-Uncensored
  ```

  

  