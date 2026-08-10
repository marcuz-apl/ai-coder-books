# Google Antigravity, Agent Skills, NotebookLM



## Part 1 - AI Agent Installation



### 1. Install Antigravity CLI, 2.0, and IDE

The Antigravity CLI binary operates under the `agy` execution command. Open your terminal or command line app and run the respective command below based on your operating system: [[1](https://antigravity.google/docs/cli-install), [2](https://www.youtube.com/watch?v=zRP9jAKNRrs), [3](https://pasqualepillitteri.it/en/news/3422/antigravity-cli-agy-install-migrate-gemini-cli)]

- **macOS / Linux**:

  ```bash
  curl -fsSL https://antigravity.google/cli/install.sh | bash
  ```

- **Windows (PowerShell)**:

  ```powershell
  irm https://antigravity.google/cli/install.ps1 | iex
  ```

- **Windows (Command Prompt)**:

  ```cmd
  curl -fsSL https://antigravity.google/cli/install.cmd -o install.cmd && install.cmd && del install.cmd
  ```

  *Note: If your terminal throws an error stating that `agy` is not recognized after installation, you must manually add the binary path (e.g., `~/.local/bin/agy` for Unix or `AppData\Local\agy\bin` for Windows) to your system’s environmental `PATH` variables.*

### 2. Install Antigravity 2.0

1. Navigate to the official [Google Antigravity Download Page](https://antigravity.google/download).
2. Download the [macOS Apple Silicon](https://storage.googleapis.com/antigravity-public/antigravity-hub/2.2.1-5287492581195776/darwin-arm/Antigravity.dmg) or [Windows X64](https://storage.googleapis.com/antigravity-public/antigravity-hub/2.2.1-5287492581195776/windows-x64/Antigravity-x64.exe) or [Linux x64](https://storage.googleapis.com/antigravity-public/antigravity-hub/2.2.1-5287492581195776/linux-x64/Antigravity.tar.gz)
3. Install it.

### 3. Install Antigravity IDE

The **Antigravity IDE** is a standalone, AI-first fork of Visual Studio Code. Follow these steps to install it on your system

#### Windows & macOS

1. Navigate to the official [Google Antigravity Download Page](https://antigravity.google/download).
2. Click the installer download link for [macOS Apple Silicon](https://edgedl.me.gvt1.com/edgedl/release2/j0qc3/antigravity/stable/2.1.1-6123990880747520/darwin-arm/Antigravity%20IDE.dmg) or [Windows X64](https://edgedl.me.gvt1.com/edgedl/release2/j0qc3/antigravity/stable/2.1.1-6123990880747520/windows-x64/Antigravity%20IDE.exe) or [Linux x64](https://edgedl.me.gvt1.com/edgedl/release2/j0qc3/antigravity/stable/2.1.1-6123990880747520/linux-x64/Antigravity%20IDE.tar.gz)
3. Run the downloaded setup executable (`.exe` or `.dmg`).
4. Accept the license terms, select your installation path, check the boxes to **Add to Path**, and click **Install**.

#### Linux (Debian / Ubuntu / RPM)

For Debian/Ubuntu-based distributions, add the repository and install via your package manager: 

```bash
sudo tee /etc/apt/sources.list.d/antigravity.list << EOL
deb [trusted=yes] https://pkg.dev stable main
EOL
sudo apt update && sudo apt install antigravity
```

For RPM-based distributions (Red Hat, Fedora, SUSE):

```bash
sudo tee /etc/yum.repos.d/antigravity.repo << EOL
[antigravity-rpm]
name=Antigravity RPM Repository
baseurl=https://us-central1-yum.pkg.dev/projects/antigravity-auto-updater-dev/antigravity-rpm
enabled=1
gpgcheck=0
EOL
sudo dnf makecache && sudo dnf install antigravity
```

### 4. Verify & Authenticate

1. Open a new terminal window.
2. Type **`agy`** (for CLI verification) or **`anti-gravity`** (for IDE workspace routing) and press **Enter**.
3. The system will automatically prompt you to log in via a silent browser redirect.
4. Log into your **Google Account** and grant permissions to complete the setup. 

If you want to customize your setup further, let me know your **operating system**, whether you are configuring an **enterprise environment**, or if you intend to sync settings from an existing **VS Code / Cursor setup**.



## Part 2 - Agent Skills



Agent Skills for Google products and technologies



This repository contains [Agent Skills](https://agentskills.io/home) for Google products and technologies, including [Google Cloud](https://cloud.google.com/).

Note: This repository is under active development.

## Installation

```
npx skills add google/skills
```

From the `npx install` command, you can select the specific skills from this repo to install.



## Resource

https://github.com/google/skills