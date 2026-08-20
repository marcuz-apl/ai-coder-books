# Qwen3.8-27B Local Setup Guide

## Dell Precision 7820 · Windows 11 IoT Enterprise LTSC · WSL2 Ubuntu 24.04 · 2× Quadro P5000

This is a future-reference guide for running a quantized **Qwen3.8-27B** model locally on a Dell Precision 7820 Tower with two NVIDIA Quadro P5000 GPUs.

The recommended arrangement is:

1. Keep Windows 11 IoT Enterprise LTSC as the host operating system.
2. Use LM Studio on Windows for the first GUI-based installation and dual-GPU test.
3. Use WSL2 Ubuntu 24.04 for the controllable command-line `llama.cpp` installation.
4. Add Ollama later only if its workflow or ecosystem is specifically useful.

Do not install every runtime at once. Validate the NVIDIA and WSL foundation first, then add one inference layer at a time.

## Hardware baseline

| Component | System |
|---|---|
| Workstation | Dell Precision 7820 Tower |
| Operating system | Windows 11 IoT Enterprise LTSC |
| CPUs | 2× Intel Xeon Silver 4116 |
| CPU topology | 24 physical cores / 48 threads total |
| System RAM | 256 GB |
| GPUs | 2× NVIDIA Quadro P5000 |
| GPU memory | 16 GB VRAM per GPU, 32 GB aggregate |
| Storage | 4× approximately 1.82 TB HDD |

The two P5000s do not behave like one 32 GB graphics card. `llama.cpp` must distribute model layers or tensors across two separate CUDA devices, with some synchronization overhead between them.

For this model size, start with a 4-bit GGUF quantization. The two GPUs should provide enough aggregate VRAM for the model plus working memory, especially at an initial 8K context size.

### Practical expectations

Expected generation speed is hardware- and build-dependent. A rough target for this older dual-GPU setup is approximately **10–18 tokens/second**, but treat that as an estimate rather than a guarantee. Measure the actual result with the benchmark commands in this guide.

The P5000s are Pascal-generation cards. Their main limitation is memory bandwidth and age, not the amount of aggregate VRAM. A fast NVMe SSD is strongly preferred for model loading; the existing HDD array is better used for archives and infrequently used models.

## Recommended architecture

```text
                    Dell Precision 7820 Tower
                              │
             Windows 11 IoT Enterprise LTSC host
                              │
        ┌─────────────────────┴─────────────────────┐
        │                                           │
 NVIDIA Windows driver                         WSL2 Ubuntu 24.04
        │                                           │
  ┌─────┴─────┐                              CUDA access via WSL
  │           │                                           │
P5000 #0   P5000 #1                              llama.cpp build
  │           │                                           │
  └─────┬─────┘                              Qwen3.8-27B GGUF
        │                                           │
   LM Studio GUI                              llama-server
   (Windows)                                         │
                                                    │
                                    OpenAI-compatible local API
                                      http://localhost:8080
```

Optional later:

```text
Ollama ──► convenient model/API workflow
```

Windows applications can generally reach a service listening in WSL through `localhost`, so the WSL inference server can remain transparent to Windows tools.

## Installation order

Use this order:

1. Verify both GPUs in Windows PowerShell.
2. Verify both GPUs inside WSL2 Ubuntu 24.04.
3. Do **not** install a second NVIDIA display driver inside WSL.
4. Install LM Studio natively on Windows.
5. Download a Qwen3.8-27B GGUF and test both GPUs in LM Studio.
6. Put frequently used models on an SSD/NVMe drive.
7. Build CUDA-enabled `llama.cpp` inside WSL.
8. Benchmark automatic allocation and an explicit `1,1` GPU split.
9. Run `llama-server` when the best settings are known.
10. Install Ollama only if it provides a workflow you actually need.

## Phase 0 — Storage and preparation

If possible, install or dedicate a 1–2 TB NVMe SSD for active models:

```text
Windows / WSL system      → SSD/NVMe
Frequently used GGUFs     → SSD/NVMe
Archives and bulk storage  → existing HDD array
```

Once a model is loaded into VRAM, storage speed does not determine generation speed. It does determine how long each model load takes.

Inside WSL, prefer the Linux filesystem for active model files:

```bash
mkdir -p ~/models/qwen/Qwen3.8-27B
```

Avoid using `/mnt/c/Models/...` for performance-sensitive model access if the model can fit in the WSL virtual disk. Access through `/mnt/c` crosses the Windows/Linux filesystem boundary and can be slower.

## Phase 1 — Verify Windows NVIDIA support

Open **PowerShell** on Windows and run:

```powershell
nvidia-smi
nvidia-smi -L
```

You want to see two Quadro P5000 devices, each with approximately 16 GB of VRAM. `nvidia-smi -L` should enumerate both devices and their UUIDs.

A useful monitoring command during later tests is:

```powershell
nvidia-smi -l 1
```

If either GPU is missing, stop here and fix the Windows NVIDIA driver/device issue before installing LM Studio, CUDA packages, or `llama.cpp`.

## Phase 2 — Verify CUDA access inside WSL2

Start Ubuntu 24.04 under WSL2 and run:

```bash
nvidia-smi
```

The same two GPUs should be visible. You can also inspect the WSL-provided CUDA interface:

```bash
ls -l /usr/lib/wsl/lib/libcuda.so*
```

Important: **do not install a separate Linux NVIDIA display driver inside WSL.** WSL2 exposes the CUDA interface from the Windows host driver. Installing a conflicting Linux display driver can make the setup harder to diagnose.

Before compiling `llama.cpp`, check whether the CUDA compiler is already available:

```bash
nvcc --version
which nvcc
```

If `nvidia-smi` works but `nvcc` is unavailable, that means the WSL CUDA runtime is visible but the CUDA development toolkit may not be installed. Confirm the exact toolkit/driver compatibility for the current environment before adding packages.

## Phase 3 — Install and test LM Studio on Windows

Install LM Studio as a native Windows application, not inside WSL:

- [LM Studio](https://lmstudio.ai/)

Search for a current Qwen3.8-27B GGUF build. Good starting candidates from the preceding procedure are:

```text
ggml-org/Qwen3.8-27B-GGUF       Q4_K_M
unsloth/Qwen3.8-27B-GGUF       UD-Q4_K_XL
```

Model repositories and filenames can change. Verify the repository and quantization label in the current model page before downloading.

### Initial LM Studio settings

Start conservatively:

```text
Model quantization:        Q4_K_M or UD-Q4_K_XL
Context length:            8192
GPU 0:                     Enabled
GPU 1:                     Enabled
GPU offload:               Maximum / 100%
GPU allocation:            Even, if available
Flash Attention:           Test enabled and disabled
KV cache:                  GPU, if VRAM allows
```

Do not start with a 128K or 256K context. The KV cache grows with context length and can consume the VRAM needed by the model and compute buffers. Increase context later in stages:

```text
8K → 16K → 32K
```

### Validate both GPUs

While LM Studio is generating text, monitor Windows from a second PowerShell window:

```powershell
nvidia-smi -l 1
```

Healthy dual-GPU behavior should show meaningful VRAM allocation on both P5000s. Exact values vary, but a result resembling this is reasonable:

```text
GPU 0: 10–14 GB used, high utilization
GPU 1: 10–14 GB used, high utilization
```

If one device has almost all the memory and the other is nearly empty, adjust LM Studio's allocation strategy. The goal is not necessarily a mathematically perfect 50/50 split; the goal is stable inference with both devices contributing.

LM Studio is the best first test because it exposes GPU allocation controls without requiring command-line tuning. It uses a `llama.cpp`-based inference engine, so a successful LM Studio test is a useful validation of the model and GPU foundation.

## Phase 4 — Build CUDA-enabled llama.cpp in WSL

Install the basic build dependencies in Ubuntu:

```bash
sudo apt update
sudo apt install -y \
    build-essential \
    cmake \
    git \
    curl \
    libcurl4-openssl-dev
```

Clone the official repository:

```bash
cd ~
git clone https://github.com/ggml-org/llama.cpp
cd llama.cpp
```

Configure and build with CUDA enabled:

```bash
cmake -B build \
    -DGGML_CUDA=ON \
    -DCMAKE_BUILD_TYPE=Release

cmake --build build --config Release -j
```

The resulting tools are typically under `build/bin/`, including:

```text
llama-cli
llama-server
llama-bench
```

If CMake cannot find CUDA, do not randomly install multiple driver packages. First confirm:

```bash
nvidia-smi
nvcc --version
```

Then install or repair only the compatible CUDA development toolkit for the WSL distribution and host driver.

## Phase 5 — Place and identify the model

A preferred WSL layout is:

```text
/home/<user>/models/
└── qwen/
    └── Qwen3.8-27B/
        └── Qwen3.8-27B-Q4_K_M.gguf
```

For the commands below, this guide assumes:

```bash
MODEL=~/models/qwen/Qwen3.8-27B/Qwen3.8-27B-Q4_K_M.gguf
```

Replace the filename with the exact file you downloaded. Confirm it exists before benchmarking:

```bash
ls -lh "$MODEL"
```

If you use `UD-Q4_K_XL`, update the path and keep the rest of the procedure the same.

## Phase 6 — Benchmark both P5000s

Run a baseline benchmark with maximum GPU layer offload:

```bash
cd ~/llama.cpp

./build/bin/llama-bench \
    -m "$MODEL" \
    -ngl 99
```

`-ngl 99` asks `llama.cpp` to offload as many layers as possible to the GPUs. The actual number of layers offloaded depends on available memory.

For two identical P5000s, test an explicit layer split:

```bash
./build/bin/llama-bench \
    -m "$MODEL" \
    -ngl 99 \
    --split-mode layer \
    --tensor-split 1,1
```

Do not assume `1,1` is optimal. Compare it with automatic allocation and record the results.

### Suggested test matrix

Keep the model, build, context, and prompt settings constant while testing:

| Test | Split setting | Purpose |
|---|---|---|
| A | Automatic/default | Establish baseline |
| B | `--split-mode layer --tensor-split 1,1` | Test even allocation |
| C | A slightly uneven tensor split | Test whether one device is limiting the other |

Record at least:

```text
Prompt processing:  tokens/second
Generation:        tokens/second
GPU 0 VRAM used:   GB
GPU 1 VRAM used:   GB
Context length:    tokens
Quantization:      exact GGUF label
```

While each test runs, use another Windows PowerShell window:

```powershell
nvidia-smi -l 1
```

Stop and reduce the context length if you see out-of-memory errors, instability, or excessive system-RAM fallback.

## Phase 7 — Run llama-server as a local API

Once the best GPU settings are known, start the server. This is a representative starting command:

```bash
cd ~/llama.cpp

./build/bin/llama-server \
    -m "$MODEL" \
    -ngl 99 \
    --split-mode layer \
    --tensor-split 1,1 \
    -c 8192 \
    --host 0.0.0.0 \
    --port 8080
```

If automatic allocation benchmarks better, omit the explicit split options. If 8K is stable, try 16K and then 32K while watching VRAM.

The resulting arrangement is:

```text
Qwen3.8-27B GGUF
        │
2× Quadro P5000
        │
llama.cpp in WSL Ubuntu 24.04
        │
llama-server :8080
        │
OpenAI-compatible local API
        │
http://localhost:8080
```

The server can then be used by local browser UIs, Python applications, scripts, and other tools that support an OpenAI-compatible endpoint.

## Optional API smoke test

With `llama-server` running, test the health endpoint from WSL or Windows:

```bash
curl http://localhost:8080/health
```

Then inspect the API root or models endpoint if supported by the build:

```bash
curl http://localhost:8080/v1/models
```

A minimal chat request is typically:

```bash
curl http://localhost:8080/v1/chat/completions \
    -H "Content-Type: application/json" \
    -d '{
      "model": "local-qwen",
      "messages": [
        {"role": "user", "content": "Briefly explain what WSL2 provides."}
      ],
      "temperature": 0.7,
      "max_tokens": 128
    }'
```

The exact accepted model name and endpoint behavior can vary with the `llama.cpp` version, so use the server's current help output if an option has changed:

```bash
./build/bin/llama-server --help
```

## Optional Phase 8 — Add Ollama later

Ollama is convenient for a simple CLI and API workflow, but it is not required if `llama-server` already meets your needs.

For this dual-P5000 workstation, the preferred order remains:

```text
LM Studio → llama.cpp under WSL → Ollama later
```

The reason is diagnostic control. LM Studio makes GPU allocation visible, while standalone `llama.cpp` exposes the split and benchmark parameters directly. Ollama can then be added after the hardware and model are known to work.

When using Ollama, consult its current Windows and multi-GPU documentation for the supported model-import and GPU-selection syntax. Do not assume that an Ollama model tag or environment variable is identical across Windows and WSL releases.

For a local GGUF import, the general pattern is a `Modelfile` pointing at the GGUF file, followed by:

```text
ollama create <local-name> -f Modelfile
ollama run <local-name>
```

Use the current Ollama documentation for the exact `FROM` path format and GPU-selection behavior.

## Troubleshooting checklist

### Only one GPU appears

Check in this order:

```powershell
nvidia-smi -L
```

Then inside WSL:

```bash
nvidia-smi
```

If Windows itself cannot see both GPUs, fix the host driver, device seating, power, or hardware issue first. If Windows sees both but WSL sees one, investigate WSL2 and the Windows driver integration before changing `llama.cpp` parameters.

### Model does not fit

Try these changes in order:

1. Confirm both GPUs are enabled.
2. Use maximum GPU offload.
3. Start with an 8K context.
4. Try an even `1,1` split.
5. Try a smaller quantization such as Q3 if necessary.
6. Leave a small amount of VRAM free for compute buffers and the KV cache.

### The second GPU has very little activity

Check the allocation strategy rather than immediately changing the model. In LM Studio, test even allocation. In `llama.cpp`, compare automatic allocation with:

```text
--split-mode layer --tensor-split 1,1
```

Remember that utilization can fluctuate even when both GPUs are contributing. VRAM allocation and repeatable benchmark results are more useful than a single instantaneous utilization reading.

### The model loads slowly

Move the active GGUF from the HDD array to an NVMe SSD. Loading a 17–25 GB model from a spinning disk can be noticeably slower even though the eventual generation speed is mostly determined by GPU memory bandwidth and synchronization.

### CUDA compilation fails

Capture the outputs of:

```bash
nvidia-smi
nvcc --version
cmake --version
```

Then verify that the CUDA toolkit used for compilation is compatible with the Windows NVIDIA driver exposed to WSL. Avoid installing a second Linux display driver inside WSL.

## Useful commands at a glance

### Windows PowerShell

```powershell
nvidia-smi
nvidia-smi -L
nvidia-smi -l 1
```

### WSL Ubuntu

```bash
nvidia-smi
nvcc --version
ls -l /usr/lib/wsl/lib/libcuda.so*
```

### Build llama.cpp

```bash
cd ~/llama.cpp
cmake -B build -DGGML_CUDA=ON -DCMAKE_BUILD_TYPE=Release
cmake --build build --config Release -j
```

### Benchmark

```bash
./build/bin/llama-bench -m "$MODEL" -ngl 99
```

### Explicit two-GPU split

```bash
./build/bin/llama-bench \
    -m "$MODEL" \
    -ngl 99 \
    --split-mode layer \
    --tensor-split 1,1
```

## Recommended final state

```text
Windows 11 IoT Enterprise LTSC
│
├── NVIDIA Windows driver
│   ├── Quadro P5000 #0
│   └── Quadro P5000 #1
│
├── LM Studio
│   └── GUI testing and model management
│
└── WSL2 Ubuntu 24.04
    ├── CUDA access through the Windows driver
    ├── llama.cpp
    ├── Qwen3.8-27B GGUF on SSD/NVMe
    └── llama-server on localhost:8080

Optional:
└── Ollama for a separate convenience/API workflow
```

The immediate next action is intentionally small: run `nvidia-smi` once in Windows PowerShell and once inside WSL Ubuntu 24.04. If both outputs show both P5000s, proceed to LM Studio and the staged model test.

## Links and resources

- [NVIDIA CUDA on WSL User Guide](https://docs.nvidia.com/cuda/wsl-user-guide/index.html)
- [Microsoft WSL networking documentation](https://learn.microsoft.com/en-us/windows/wsl/networking)
- [llama.cpp GitHub repository](https://github.com/ggml-org/llama.cpp)
- [LM Studio](https://lmstudio.ai/)
- [Ollama](https://ollama.com/)
- [Dell Precision 7820 Tower support](https://www.dell.com/support/home/en-us/product-support/product/precision-7820-workstation/overview)

Always verify current model repositories, quantization filenames, CUDA compatibility, LM Studio controls, and Ollama syntax before applying this guide to a newly updated installation.
