# Google Antigravity CLI, 2.0, and IDE



## Part 1 - Antigravity CLI

Work with Antigravity directly in your codebase. Build, debug, and ship from your terminal. Describe what you need, and Antigravity handles the rest.

### macOS | Linux
```shell
curl -fsSL https://antigravity.google/cli/install.sh | bash
```
### Windows PowerShell
```shell
irm https://antigravity.google/cli/install.ps1 | iex
```
### Windows CMD
```shell
curl -fsSL https://antigravity.google/cli/install.cmd -o install.cmd && install.cmd && del install.cmd
```



## Part 2 - Antigravity 2.0



### Deb-based Linux distributions (e.g. Debian, Ubuntu)

#### 1. Add the repository to sources.list.d

```
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://us-central1-apt.pkg.dev/doc/repo-signing-key.gpg | \
  sudo gpg --dearmor --yes -o /etc/apt/keyrings/antigravity-repo-key.gpg
echo "deb [signed-by=/etc/apt/keyrings/antigravity-repo-key.gpg] https://us-central1-apt.pkg.dev/projects/antigravity-auto-updater-dev/ antigravity-debian main" | \
  sudo tee /etc/apt/sources.list.d/antigravity.list > /dev/null
```

#### 2. Update the package cache

```
sudo apt update
```

#### 3. Install the package

```
sudo apt install antigravity
```



### Rpm-based Linux distributions (eg. Red Hat, Fedora, SUSE)

#### 1. Add the repository to /etc/yum.repos.d

```
sudo tee /etc/yum.repos.d/antigravity.repo << EOL
[antigravity-rpm]
name=Antigravity RPM Repository
baseurl=https://us-central1-yum.pkg.dev/projects/antigravity-auto-updater-dev/antigravity-rpm
enabled=1
gpgcheck=0
EOL
```

#### 2. Update the package cache

```
sudo dnf makecache
```

### 3. Install the package

```
sudo dnf install antigravity
```



## Part 3 - Antigravity IDE



### Linux edition

Install the Antigravity IDE as below:

```shell
## Download the installer
wget https://edgedl.me.gvt1.com/edgedl/release2/j0qc3/antigravity/stable/2.1.1-6123990880747520/linux-x64/Antigravity%20IDE.tar.gz

## Extract the installer package
sudo tar -xzf 'Antigravity IDE.tar.gz' -C /opt

## rename the folder
sudo mv /opt/'Antigravity IDE' /opt/antigravity-ide

## Configure the binary file in /opt/antigravity-ide folder
echo 'PATH="/opt/antigravity-ide/bin:$PATH"' >> ~/.profile
source ~/.profile

## Run the app
antigravity-ide
```

Then, Add Desktop shortcut for Agy IDE by creating/editing a file:

```shell
sudo nano /usr/share/applications/agy-ide.desktop
```

with content as below:

```text
[Desktop Entry]
Type=Application
Name=Agy IDE
Comment=Launch Google Antigravity IDE application
Exec=/opt/antigravity-ide/bin/antigravity-ide %U
Icon=/opt/antigravity-ide/resources/app/resources/linux/code.png
Terminal=false
Categories=Programming;
```

Then make it executable:

```shell
sudo chmod +x /usr/share/applications/antigravity-ide.desktop
```



### macOS Edition

```shell
## Download
wget https://edgedl.me.gvt1.com/edgedl/release2/j0qc3/antigravity/stable/2.1.1-6123990880747520/darwin-arm/Antigravity%20IDE.dmg
## Install with the installer
```



### Windows edition

```shell
## Download
wget https://edgedl.me.gvt1.com/edgedl/release2/j0qc3/antigravity/stable/2.1.1-6123990880747520/windows-x64/Antigravity%20IDE.exe
## Install with the installer
```

