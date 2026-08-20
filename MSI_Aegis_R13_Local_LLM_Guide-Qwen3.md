# Local Multimodal LLM Guide for the 2024 MSI Aegis R13

## Hardware profile

- **CPU:** Intel Core i7-13700F
- **GPU:** NVIDIA GeForce RTX 4060 Ti, 16 GB VRAM
- **System memory:** 32 GB DDR5
- **Storage:** 2 × 1 TB NVMe SSDs
- **Power supply:** 650 W

This is a capable local-inference system for 8B–12B multimodal models and can run a quantized 27B multimodal model, although the larger model will generally trade speed and convenience for quality.

## Bottom line

The best practical setup is a **two-model workflow**:

1. **Fast everyday model:** Qwen3-VL-8B at Q6 or Q8 for responsive image understanding, document/image questions, and general assistance.
2. **Higher-quality model:** Qwen3.8-27B at Q4 when more reasoning or answer quality matters and slower generation is acceptable.

Gemma-class 12B models are a strong middle ground if you want more capability than an 8B model without the memory and speed penalties of a 27B model.

## Qwen3.8-27B viability

Qwen3.8-27B is **viable, but not an ideal all-day model** on this machine.

A 27B dense model in a 4-bit quantization is approximately in the **15–17 GB weight range**, before accounting for the KV cache, multimodal components, runtime overhead, and the rest of the operating system. The RTX 4060 Ti has 16 GB of VRAM, so a Q4 build will usually require some combination of:

- partial CPU/RAM offloading;
- a reduced context size;
- a smaller or more aggressively compressed quantization; or
- careful runtime settings to leave enough memory for the KV cache.

The 32 GB of DDR5 makes this workable. It removes the biggest practical obstacle, but it does not make the model GPU-resident. Because CPU/RAM spill is slower than keeping all layers in VRAM, the model will feel noticeably slower than an 8B or 12B model.

### Recommended quantizations

| Quantization | Recommendation | Practical trade-off |
|---|---|---|
| **Q4_K_M** | Best starting point for quality | Usually requires some RAM spill and careful memory management |
| **IQ4_XS** | Good alternative to Q4_K_M | Similar size with a different quality/size trade-off |
| **IQ3 or other 3-bit build** | Use when speed or fit is more important | More likely to fit comfortably, but with some quality loss |
| **Q5 or higher** | Generally inconvenient on this hardware | More quality, but less memory headroom and more offloading |

Start with **Q4_K_M** or **IQ4_XS**. If the model is too slow or unstable at your preferred context length, try a good 3-bit build rather than immediately increasing system complexity.

## Estimated generation performance

The following are realistic planning estimates for this hardware, not guaranteed benchmarks. Actual rates vary with the runtime, CUDA and llama.cpp versions, flash attention, context length, image resolution, prompt length, quantization, and how many layers are offloaded to the GPU.

| Model / quantization | Expected memory behavior | Estimated generation rate | Practical feel |
|---|---|---:|---|
| **Qwen3.8-27B Q4_K_M** | Around the VRAM limit with RAM spill | **8–15 tokens/sec** | Usable, but noticeably slower |
| **Qwen3.8-27B IQ3 / 3-bit** | More comfortable VRAM/RAM balance | **12–20 tokens/sec** | Faster, with some quality loss |
| **Qwen3-VL-8B Q8** | Fully GPU-resident or very close | **25–35 tokens/sec** | Very responsive |
| **Qwen3-VL-8B Q6** | Fully GPU-resident | **30–40 tokens/sec** | Excellent balance |
| **Gemma-class 12B Q4** | Fully or nearly GPU-resident | **20–30 tokens/sec** | Responsive |
| **Gemma 3 12B Q4_K_M** | GPU-friendly | **About 28–30 tokens/sec** | Very responsive |
| **Typical 8B Q4 model** | Plenty of VRAM headroom | **35–45 tokens/sec** | Fast |

These estimates describe **generation speed after prompt processing**. Processing a large image, long document, or long context can take additional time before token generation begins.

## Model recommendations by use case

### 1. Qwen3-VL-8B — best everyday model

Use this as the default local assistant when responsiveness matters.

Good for:

- image description and visual question answering;
- screenshots, charts, and documents;
- general chat and writing assistance;
- frequent interactive use where waiting is frustrating.

Use **Q6** for the best balance, or **Q8** when you prefer maximum quality and can accept a small speed and memory cost.

### 2. Gemma-class 12B — best middle ground

A 12B-class Gemma model offers a useful step up from many 8B models while remaining much easier to run than a 27B model.

Good for:

- stronger general reasoning than a basic 8B model;
- writing, summarization, and analysis;
- users who want a responsive model without relying on CPU offloading;
- situations where multimodal quality is useful but not the only priority.

Use a **Q4_K_M** build as the normal starting point. A 12B Q4 model should be much more responsive than the 27B model on this system.

### 3. Qwen3.8-27B — quality-focused model

Use this when answer quality and reasoning depth matter more than speed.

Good for:

- difficult reasoning and multi-step analysis;
- complex image or document interpretation;
- occasional high-quality answers;
- tasks where an extra minute of waiting is acceptable.

Use **Q4_K_M** or **IQ4_XS** first. Expect the need for RAM spill and a generation rate commonly around **8–15 tokens/sec**. A 3-bit build can improve fit and speed at the cost of some quality.

## Model ranking for this PC

### Best overall workflow

1. **Qwen3-VL-8B Q6** — best balance of speed, quality, and multimodal usefulness.
2. **Gemma-class 12B Q4** — best middle tier when you want more capability while staying responsive.
3. **Qwen3.8-27B Q4** — best quality ceiling that is reasonably practical, but slowest and most memory-sensitive.

### Best by priority

| Priority | Recommended model |
|---|---|
| Fastest useful multimodal assistant | Qwen3-VL-8B Q6 or Q8 |
| Best balance | Qwen3-VL-8B Q6 |
| Stronger middle tier | Gemma-class 12B Q4 |
| Highest practical quality | Qwen3.8-27B Q4_K_M |
| Better fit/speed for the 27B model | Qwen3.8-27B IQ3 / 3-bit |

## VRAM and RAM behavior

The RTX 4060 Ti's 16 GB VRAM is the main constraint. Smaller quantized models can remain entirely on the GPU, which is why they feel much faster. A Q4 27B model is close to or larger than the available VRAM once runtime overhead and KV cache are included.

For Qwen3.8-27B, the likely behavior is:

- model weights use most or all of the GPU memory;
- some layers, the KV cache, or multimodal runtime data may use system RAM;
- the i7-13700F helps make offloading possible, but system RAM is much slower than VRAM;
- longer contexts and larger images increase memory use;
- leaving other GPU-heavy applications closed improves stability and available headroom.

The 32 GB system-memory configuration is enough to make the 27B Q4 model practical for focused use. It is less comfortable if you want a large context, several applications open, or multiple models loaded at once.

## Is 64 GB RAM necessary?

No. **64 GB is optional, not required.**

The current 32 GB configuration is sufficient for the recommended 8B and 12B models and makes a 27B Q4 model viable. Upgrading to 64 GB would provide useful headroom for:

- larger context windows;
- image and document workloads with more runtime overhead;
- keeping other desktop applications open;
- CPU-offloaded layers and larger model variants;
- running local tools alongside the LLM.

If the primary goal is speed, a GPU with more VRAM would make a larger difference than adding RAM. If the goal is smoother large-model use on the current GPU, 64 GB is a sensible quality-of-life upgrade.

## Preferred operating environments and runtimes

This machine is well suited to either **LM Studio** or a direct **llama.cpp** installation. The best choice depends mostly on whether you want a graphical workflow or a reproducible command-line/API workflow.

### Option A: Windows 11 with native LM Studio

This is the easiest starting point. Install LM Studio directly on Windows 11, download compatible GGUF model files, and use the model loader to adjust context length, GPU offload, and Flash Attention.

Recommended use:

- **Qwen3-VL-8B Q6/Q8:** use automatic or maximum GPU offload; it should be comfortable on the RTX 4060 Ti.
- **Gemma-class 12B Q4:** use automatic or maximum GPU offload and a moderate context size.
- **Qwen3.8-27B Q4_K_M:** start with automatic GPU offload. If loading fails or the context is too small, reduce the context length or use a 3-bit build.

LM Studio supports Windows and Linux, runs GGUF models through a llama.cpp runtime, and offers a GUI for downloading, loading, and chatting with models. Its `lms` command-line tool can also estimate memory before loading a model:

```text
lms load --estimate-only <model_key>
lms load <model_key> --gpu max
lms unload --all
```

For this PC, LM Studio is the most convenient way to compare Qwen3-VL-8B, Gemma-class 12B, and Qwen3.8-27B quantizations without manually changing command-line flags.

### Option B: Windows 11 + WSL2 Ubuntu 24.04 with pure llama.cpp

This is the preferred Windows setup if you want a Linux command-line environment while keeping Windows as the host operating system. Use **WSL2**, not WSL1, and make sure the NVIDIA Windows driver supports CUDA in WSL.

The broad workflow is:

1. Install or update WSL2 and an Ubuntu 24.04 distribution.
2. Confirm that the NVIDIA GPU is visible inside WSL2.
3. Install the Linux build tools and a CUDA toolkit compatible with the host NVIDIA driver.
4. Build llama.cpp with its CUDA backend enabled.
5. Store active model files in the WSL Linux filesystem, such as `~/models`, for better file access than a heavily used `/mnt/c/...` path.
6. Run `llama-cli` for terminal use or `llama-server` for a local HTTP API.

The official llama.cpp CUDA build uses CMake in this general form:

```bash
git clone https://github.com/ggml-org/llama.cpp.git
cd llama.cpp
cmake -B build -DGGML_CUDA=ON
cmake --build build --config Release
```

For a model that exceeds the 16 GB VRAM capacity, llama.cpp supports hybrid CPU+GPU inference. Start by placing as many layers as practical on the GPU, then adjust GPU layers and context length while watching both VRAM and system RAM. Use `--list-devices` when you need to inspect available backends or devices.

WSL2 is a good fit for llama.cpp because it provides Linux tooling while allowing CUDA applications to use the Windows-hosted NVIDIA driver. It is especially attractive if you want shell scripts, services, Docker, or a local OpenAI-compatible API.

### Option C: Pure Ubuntu 24.04

For a Linux-only installation, pure Ubuntu 24.04 is the cleanest environment for direct llama.cpp use:

- install the appropriate NVIDIA driver and CUDA toolkit for the system;
- build llama.cpp with `-DGGML_CUDA=ON`;
- run `llama-cli` interactively or `llama-server` as a local service;
- keep models on a fast local NVMe filesystem;
- use a system service or shell script if you want the server to start automatically.

LM Studio also provides Linux downloads, including an AppImage, and its headless `llmster` daemon can run without the desktop GUI. However, LM Studio’s current system-requirements page notes that Ubuntu versions newer than 22 are not as well tested. For Ubuntu 24.04 specifically, **pure llama.cpp is the more conservative choice**, while LM Studio/llmster is worth trying if you prefer its model management and API workflow.

### Which route I recommend

| Preference | Recommended environment | Runtime |
|---|---|---|
| Simplest GUI and model management | Windows 11 | Native LM Studio |
| Linux tools while keeping Windows | Windows 11 + WSL2 Ubuntu 24.04 | Pure llama.cpp |
| Linux-only, maximum control | Ubuntu 24.04 | Pure llama.cpp |
| Headless local API with LM Studio features | Windows or Ubuntu | `llmster` / `lms` |

For your stated preferences, I would start with **native LM Studio on Windows 11** for easy model comparison, then keep a **WSL2 Ubuntu 24.04 llama.cpp build** for scripting, benchmarking, and server/API use. If you move to pure Ubuntu 24.04, use llama.cpp as the primary runtime and treat LM Studio/llmster as an optional convenience layer.

## Suggested setup

For a simple local setup:

1. Install a CUDA-capable runtime such as llama.cpp-based software or another local model front end that supports the model format.
2. Keep **Qwen3-VL-8B Q6** as the default model.
3. Keep **Qwen3.8-27B Q4_K_M** as the quality model.
4. Use a moderate context size initially, then increase it only if memory remains stable.
5. Enable GPU offloading and flash attention when supported by the runtime.
6. Monitor VRAM, system RAM, prompt processing time, and generation speed while testing.

The most practical experience will come from switching models based on the task: use the 8B model for quick interaction and the 27B model for questions where quality is worth the wait.

## Final recommendation

This MSI Aegis R13 is a good local multimodal machine, with an important distinction:

- **8B models** are fast and comfortable.
- **12B models** are a strong responsive middle ground.
- **27B Q4 models** are viable and can provide higher quality, but they are memory-sensitive and slower because they cannot comfortably fit entirely in 16 GB of VRAM.

The recommended starting point is **Qwen3-VL-8B Q6 for everyday use**, with **Qwen3.8-27B Q4_K_M available for quality-focused tasks**. Keep the current 32 GB of RAM initially; consider 64 GB later if large contexts, heavy image/document work, or multitasking becomes a limitation.

## Official references

- [LM Studio system requirements](https://lmstudio.ai/docs/app/system-requirements)
- [LM Studio basics](https://lmstudio.ai/docs/app/basics)
- [LM Studio model loading and GPU offload](https://lmstudio.ai/docs/cli/local-models/load)
- [LM Studio headless mode and llmster](https://lmstudio.ai/docs/developer/core/headless)
- [llama.cpp build guide](https://github.com/ggml-org/llama.cpp/blob/master/docs/build.md)
- [llama.cpp project and quick start](https://github.com/ggml-org/llama.cpp)
- [NVIDIA CUDA on WSL guide](https://docs.nvidia.com/cuda/wsl-user-guide/index.html)

*All token-rate figures in this guide are estimates for planning and should be validated on the exact quantized files and runtime you choose.*
