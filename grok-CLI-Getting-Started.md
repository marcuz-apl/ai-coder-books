# Grok CLI: Getting Started

 



## Intro: Settings and Approaches

The situation is:

- Love Vibe Coding with Grok CLI
- Wanna cut cost of tokens by integrating local models



## Part 1 - Operations - Official



### 1- Pre-requisites: Installing Git/NodeJS/Python

Download Git installer from https://git-scm.com/install/ and install Git, or simply:

```shell
# Windows
winget install --id Git.Git
winget install --id OpenJS.NodeJS.LTS
winget install --id Python.Python.3.14
winget install --id Microsoft.DotNet.SDK.10
winget install --id GitHub.cli

# macOS
brew install git
# Debian/Ubuntu
apt install git
# Fedora/RHEL/Rocky
dnf install git
```



### 2- Install Grok CLI

Install `Grok CLI`  as vibe coding agent.

```shell
# Mac/Linux/WSL
curl -fsSL https://x.ai/cli/install.sh | bash

# Windows PowerShell
irm https://x.ai/cli/install.ps1 | iex
```

Grok CLI: Version check

```shell
# Version check
Grok --version
```



### 3- Start an interactive session

Simply run `Grok` to get started. Then sign in as prompted.

```shell
cd your-project
grok
```

On first launch, Grok opens a browser for authentication. In non-browser environments, use an API key:

```shell
export XAI_API_KEY="your-api-key-here"
grok
```

Useful first prompts:

```text
Explain this repo.
@src/main.rs Walk me through this file.
```





## Part 2 - Special ops

### 1- Run headlessly

```shell
grok -p "Explain this codebase"
grok -p "Explain the architecture" --output-format streaming-json
```

Headless usage is ideal for scripts, automations, or integration into other apps.

[Custom models](https://docs.x.ai/build/overview#custom-models)

Grok supports any custom model. Add it to your user-level config file, `~/.grok/config.toml` (on Windows, `%USERPROFILE%\.grok\config.toml`):

```shell
[model.my-model]
model = "model-id"
base_url = "https://api.example.com/v1"
name = "Display Name"
env_key = "API_KEY"

[models]
default = "my-model"
```

After updating `~/.grok/config.toml`, use `grok inspect` to see what Grok discovered in the current directory, including config sources, instructions, skills, plugins, hooks, and MCP servers, then pick the model in headless mode or in the TUI:

```shell
grok inspect
grok -p "Hello" -m my-model
```

You can also switch inside the TUI with `/model <name>`.



### 2- Use Grok 4.5 on the API

The same model that powers Grok Build, [`grok-4.5`](https://docs.x.ai/developers/models/grok-4.5), is also available directly on the xAI API. Drop it into your own agent loop, IDE integration, or coding tool.

bash

```bash
curl https://api.x.ai/v1/responses \
  -H "Authorization: Bearer $XAI_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "grok-4.5",
    "input": "Fix this function and explain the bug: function median(a){a.sort();return a[a.length/2]}"
  }'
```

python:

```python
import os
from xai_sdk import Client
from xai_sdk.chat import user

client = Client(api_key=os.getenv("XAI_API_KEY"))

chat = client.chat.create(model="grok-4.5")
chat.append(user("Fix this function and explain the bug: function median(a){a.sort();return a[a.length/2]}"))

print(chat.sample().content)
```

Python (OpenAI)

```shell
from openai import OpenAI

client = OpenAI(
    api_key="<YOUR_XAI_API_KEY_HERE>",
    base_url="https://api.x.ai/v1",
)

response = client.responses.create(
    model="grok-4.5",
    input="Fix this function and explain the bug: function median(a){a.sort();return a[a.length/2]}",
)

print(response.output_text)
```

JavaScript:

```shell
import { xai } from '@ai-sdk/xai';
import { generateText } from 'ai';

const { text } = await generateText({
  model: xai.responses('grok-4.5'),
  prompt: 'Fix this function and explain the bug: function median(a){a.sort();return a[a.length/2]}',
});

console.log(text);
```

### 3- Uninstall grok

#### Method 1: Using the CLI's Built-in Command (Recommended)

```shell 
## Method 1: Using the CLI's Built-in Command (Recommended)
grok uninstall
```

```text
Flags:
	--keep-config: Keeps your user configuration, skills, and memory in ~/.grok while removing the binaries.
	--dry-run: Preview what files will be removed without deleting anything.
```

#### Method 2: Manual Removal (macOS / Linux / WSL)

If `grok uninstall` isn't supported by your version or you installed via `curl | bash`, remove the binaries and directory manually:

```shell
## 1- Remove the binaries/symlinks:
sudo rm -f /usr/local/bin/grok /usr/local/bin/agent
```

*(If binaries were placed in your user PATH instead: `rm -f ~/.grok/bin/grok ~/.grok/bin/agent`)*

```shell
## 2- Remove configuration files & cached data:
rm -rf ~/.grok
```

```shell
## 3- Clean shell profiles (Optional):
## Open ~/.zshrc, ~/.bashrc, or ~/.bash_profile and remove any lines containing # grok or PATH exports pointing to ~/.grok/bin.
```

#### Method 3: npm / Node.js

If you installed a Node-based wrapper or global package via npm:

```shell
npm uninstall -g grok-cli
## if installed via community packages
npm uninstall -g grok-cli-hurry-mode
```



#### Special: Uninstall in PowerShell

##### Step 1: Remove the CLI Binaries

If installed via PowerShell / Windows script or manual setup, delete the binary executable files located in your user profile:

PowerShell

```powershell
Remove-Item -Path "$env:USERPROFILE\.grok\bin\*" -Force -ErrorAction SilentlyContinue
```

*(Note: If installed globally via **npm**, uninstall it using `npm uninstall -g grok-cli` instead).*

------

##### Step 2: Delete Configuration Files & Local Data

Remove your cached data, configuration settings, and local profile directory:

PowerShell

```powershell
Remove-Item -Path "$env:USERPROFILE\.grok" -Recurse -Force
```

------

##### Step 3: Remove PATH Environment Variables

If the installer added Grok to your environment PATH, remove the entry so PowerShell stops recognizing the `grok` command:

PowerShell

```powershell
# Get current User PATH
$UserPath = [Environment]::GetEnvironmentVariable("Path", "User")

# Filter out Grok paths
$NewPath = ($UserPath -split ';' | Where-Object { $_ -notlike "*\.grok*" }) -join ';'

# Save updated PATH back to User Environment Variables
[Environment]::SetEnvironmentVariable("Path", $NewPath, "User")
```

After running these steps, restart your PowerShell session to apply the PATH updates.
