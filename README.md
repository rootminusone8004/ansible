# ⚡ Personal Ansible Automation Hub 🌍

<p align="center">
  <img src="https://img.shields.io/badge/Ansible-v2.13%2B-EE0000?style=for-the-badge&logo=ansible&logoColor=white" alt="Ansible" />
  <img src="https://img.shields.io/badge/Python-3.9%2B-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/Linux-Debian%20%7C%20Ubuntu%20%7C%20RHEL-FCC624?style=for-the-badge&logo=linux&logoColor=black" alt="Linux" />
  <img src="https://img.shields.io/badge/Arch-x86__64%20%7C%20ARM64-4169E1?style=for-the-badge&logo=arm&logoColor=white" alt="Architectures" />
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License" />
</p>

---

## 📖 Table of Contents

- [✨ Highlights](#-highlights)
- [🏗️ Repository Architecture](#️-repository-architecture)
- [🧩 Roles Catalog](#-roles-catalog)
- [📜 Playbooks Overview](#-playbooks-overview)
- [🏷️ Tags Cheat Sheet](#️-tags-cheat-sheet)
- [🚀 Quick Start](#-quick-start)
  - [1. Clone the Repository](#1-clone-the-repository)
  - [2. Configure Your Inventory](#2-configure-your-inventory)
  - [3. Execute Playbooks](#3-execute-playbooks)
- [⚙️ Configuration & Overrides](#️-configuration--overrides)
- [🧪 Testing & Verification](#-testing--verification)
- [🤝 Contributing](#-contributing)
- [⚖️ License](#️-license)

---

## ✨ Highlights

- 🧱 **Atomic & Modular Roles**: Each tool and component is isolated in its own role (`roles/<name>`) with its own defaults, tasks, and handlers.
- 🔄 **Idempotent & Event-Driven**: Leverages Ansible handlers for service reloads (`containerd`, `docker`, `sysctl`) to eliminate unnecessary service restarts.
- 💻 **Multi-Architecture Ready**: Automatically resolves binary releases and package repos for both `x86_64` (AMD64) and `ARM64` / `aarch64` (including Raspberry Pi).
- 🏷️ **Granular Tagging**: Run entire development environments or isolate specific tools (`--tags zsh`, `--tags neovim`, `--tags minimal`).
- 🛡️ **Modern & Future-Proof**: Fully compliant with `ansible-core` (explicit `ansible_facts[...]` dictionary access, FQCN `ansible.builtin.*`, native YAML formatting).

---

## 🏗️ Repository Architecture

```bash
.
├── ⚙️ ansible.cfg                 # Global defaults, roles path, & YAML output formatting
├── 📂 inventory/                  # Target hosts and environment definitions
│   ├── 📄 hosts.yaml              # Default production / remote host inventory
│   ├── 📄 kubeadm.yaml            # Multi-node Kubernetes cluster inventory
│   ├── 📄 vagrant.yaml            # Local VM testing inventory
│   └── 📂 group_vars/
│       └── 📄 all.yaml            # Global variable overrides across inventories
├── 📂 playbooks/                  # Executable playbooks
│   ├── 🚀 site.yaml               # Master site orchestration
│   ├── 💻 basic_setup.yaml        # Full workstation & development environment
│   ├── 🐳 docker.yaml             # Docker Engine + LazyDocker
│   ├── 🔀 expose_ports.yaml       # Alias for port_forward.yaml
│   ├── 🛡️ kali.yaml               # Kali Linux security toolkit & configurations
│   ├── ☸️ kind.yaml               # Kubernetes-in-Docker setup
│   ├── 🌐 kubeadm.yaml            # Multi-node production-grade K8s cluster
│   ├── 📦 minikube.yaml           # Local Minikube development cluster
│   └── 🔀 port_forward.yaml       # Expose localhost ports to public IP via Nginx
└── 📂 roles/                      # Self-contained, reusable Ansible roles
    ├── 🧰 common/                 # System upgrade, EPEL repo, base utilities
    ├── 🐳 docker/                 # Container runtime, keyrings, daemon handlers
    ├── 📁 dotfiles/               # GNU Stow symlink manager & dotfile repo sync
    ├── 🔍 fzf/                    # FZF fuzzy finder (native package / release fallback)
    ├── 🛡️ kali/                   # Security auditing packages, Tmux resurrect, GRC
    ├── ☸️ kind/                   # KinD binary installer (multi-arch)
    ├── 🌐 kubeadm/                # Node preparation & control-plane initialization
    ├── 🎯 kubectl/                # Kubernetes CLI binary installer (multi-arch)
    ├── 📊 lazydocker/             # LazyDocker TUI manager (multi-arch)
    ├── 🌿 lazygit/                # LazyGit TUI git client (multi-arch)
    ├── 📦 minikube/               # Minikube binary installer (multi-arch)
    ├── 📝 neovim/                 # Neovim binary, dependencies, PATH injection
    ├── 🔀 port_forward/           # Flexible Nginx reverse proxy for localhost services
    ├── 🪟 tmux/                   # Tmux terminal multiplexer & TPM plugins
    └── 🐚 zsh/                    # Zsh shell, Powerlevel10k, syntax & autosuggest plugins
```

---

## 🧩 Roles Catalog

| Role | Description | Default Version / Settings | Handlers |
| :--- | :--- | :--- | :---: |
| [**`common`**](roles/common) | Base utilities (`gcc`, `curl`, `wget`, `btop`, etc.) & system updates | `common_upgrade_packages: true` | — |
| [**`docker`**](roles/docker) | Docker Engine, containerd, APT repository & group permissions | `docker_install_engine: true` | 🔄 Restart Docker / Containerd |
| [**`dotfiles`**](roles/dotfiles) | GNU Stow installer & dotfiles sync with automated backups | Auto-branch (`raspi` vs `server`) | — |
| [**`fzf`**](roles/fzf) | FZF fuzzy finder installer (package manager or GitHub tarball) | `v0.67.0` | — |
| [**`kali`**](roles/kali) | Pentesting suite (`nmap`, `gobuster`, `metasploit`) & tmux resurrect | Predefined security tools list | — |
| [**`kind`**](roles/kind) | KinD (Kubernetes-in-Docker) binary installer | `v0.22.0` | — |
| [**`kubeadm`**](roles/kubeadm) | K8s node prep (sysctl, swap, containerd) & control-plane init (Helm, Calico) | `v1.34` / `CIDR: 10.244.0.0/16` | 🔄 Reload sysctl / Containerd |
| [**`kubectl`**](roles/kubectl) | Kubernetes command-line client (`kubectl`) | `v1.30.0` | — |
| [**`lazydocker`**](roles/lazydocker) | Terminal UI for Docker and Docker Compose | `v0.24.1` | — |
| [**`lazygit`**](roles/lazygit) | Simple terminal UI for git commands | `v0.56.0` | — |
| [**`minikube`**](roles/minikube) | Local Kubernetes development environment | `latest` | — |
| [**`neovim`**](roles/neovim) | Neovim text editor binary & environment PATH setup | `v0.12.3` | — |
| [**`port_forward`**](roles/port_forward) | Dynamic Nginx reverse proxy to expose localhost ports to public IP | `port_forward_ports: [4512, 8888]` | 🔄 Restart / Reload Nginx |
| [**`tmux`**](roles/tmux) | Tmux multiplexer and Tmux Plugin Manager (TPM) | `TPM: master` | — |
| [**`zsh`**](roles/zsh) | Zsh shell with Powerlevel10k, syntax-highlighting & autosuggestions | `Default Shell: /usr/bin/zsh` | — |

---

## 📜 Playbooks Overview

| Playbook | Purpose | Included Roles |
| :--- | :--- | :--- |
| [`site.yaml`](playbooks/site.yaml) | 🌟 Master orchestration entry point | Imports `basic_setup.yaml`, `docker.yaml` |
| [`basic_setup.yaml`](playbooks/basic_setup.yaml) | 💻 Workstation & server environment | `common`, `zsh`, `tmux`, `fzf`, `neovim`, `lazygit`, `dotfiles` |
| [`docker.yaml`](playbooks/docker.yaml) | 🐳 Containerized host setup | `docker`, `lazydocker` |
| [`port_forward.yaml`](playbooks/port_forward.yaml) | 🔀 Dynamic localhost port exposition via Nginx | `port_forward` |
| [`expose_ports.yaml`](playbooks/expose_ports.yaml) | 🔀 Alias for `port_forward.yaml` | Imports `port_forward.yaml` |
| [`kali.yaml`](playbooks/kali.yaml) | 🛡️ Offensive security & auditing system | `common`, `kali` |
| [`kind.yaml`](playbooks/kind.yaml) | ☸️ KinD development machine | `docker`, `kubectl`, `kind` |
| [`minikube.yaml`](playbooks/minikube.yaml) | 📦 Minikube development machine | `docker`, `kubectl`, `minikube` |
| [`kubeadm.yaml`](playbooks/kubeadm.yaml) | 🌐 Production multi-node K8s cluster | `docker` (containerd only), `kubeadm` (node & control-plane) |

---

## 🏷️ Tags Cheat Sheet

Fine-tune your playbook runs using Ansible `--tags` or `--skip-tags`:

| Tag | Target | Description |
| :--- | :--- | :--- |
| `minimal` | ⚡ Core Essentials | Runs `common`, `tmux`, `fzf`, and `dotfiles` |
| `common` | 🧰 Base System | Runs OS updates, EPEL repo setup, and installs base utilities |
| `zsh` | 🐚 Shell | Installs Zsh and configures plugins (p10k, autosuggestions, etc.) |
| `neovim` | 📝 Editor | Downloads Neovim release and updates PATH |
| `tmux` | 🪟 Multiplexer | Installs tmux and clones TPM |
| `fzf` | 🔍 Fuzzy Search | Installs FZF via repo or binary |
| `lazygit` | 🌿 Git TUI | Installs LazyGit binary |
| `dotfiles` | 📁 Dotfiles | Synchronizes dotfiles repository via GNU Stow |
| `docker` | 🐳 Docker Engine | Configures Docker CE, containerd, and user permissions |
| `lazydocker`| 📊 Docker TUI | Installs LazyDocker binary |
| `node` | 🖥️ K8s Node | Configures sysctl, swapoff, and k8s binaries |
| `control_plane` | 👑 K8s Master | Runs `kubeadm init`, installs Helm, and deploys Calico CNI |

---

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/rootminusone8004/ansible.git
cd ansible
```

### 2. Configure Your Inventory

Edit the target host in [`inventory/hosts.yaml`](inventory/hosts.yaml):

```yaml
all:
  hosts:
    your.server.ip.here:
      ansible_user: admin
```

### 3. Execute Playbooks

> [!TIP]
> Add `--ask-become-pass` (or `-K`) if your user requires `sudo` password authentication.

```bash
# 🖥️ Run complete workstation setup
ansible-playbook playbooks/basic_setup.yaml

# ⚡ Run only minimal essentials
ansible-playbook playbooks/basic_setup.yaml --tags minimal

# 🎯 Run only specific tools (e.g. zsh and neovim)
ansible-playbook playbooks/basic_setup.yaml --tags zsh,neovim

# 🐳 Install Docker and LazyDocker
ansible-playbook playbooks/docker.yaml

# 🔀 Expose localhost services to public IP (default: 4512, 8888)
ansible-playbook playbooks/port_forward.yaml

# 🔀 Expose custom ports dynamically on the fly
ansible-playbook playbooks/port_forward.yaml -e 'ports="4512,8888,80:3000"'

# 🔒 Expose ports with HTTP Basic Auth password protection
ansible-playbook playbooks/port_forward.yaml -e 'auth=true -e auth_pass="MySecretPassword123!"'

# 🌐 Deploy multi-node Kubernetes cluster
ansible-playbook -i inventory/kubeadm.yaml playbooks/kubeadm.yaml

# 🧪 Run against local Vagrant box
ansible-playbook -i inventory/vagrant.yaml playbooks/basic_setup.yaml
```

---

## ⚙️ Configuration & Overrides

### 📍 Variable Precedence Hierarchy

Variables are structured cleanly with default fallbacks:
1. **Role Defaults** (`roles/<role>/defaults/main.yaml`): Lowest precedence, safe defaults.
2. **Global Overrides** ([`inventory/group_vars/all.yaml`](inventory/group_vars/all.yaml)): Applied across all inventories.
3. **Playbook Vars** (`playbooks/<playbook>.yaml`): Specific to playbooks.
4. **Extra Vars** (`-e "var_name=value"`): CLI runtime overrides (highest precedence).

### 🔧 Customizing Tool Versions

You can override tool versions globally in [`inventory/group_vars/all.yaml`](inventory/group_vars/all.yaml):

```yaml
# Tool versions
neovim_version: "v0.12.3"
lazygit_version: "0.56.0"
fzf_version: "0.67.0"
```

Or on-the-fly via the command line:

```bash
ansible-playbook playbooks/basic_setup.yaml --tags neovim -e "neovim_version=v0.12.0"
```

---

## 🧪 Testing & Verification

Check your playbooks before execution:

```bash
# 🔍 Syntax check all playbooks
ansible-playbook --syntax-check playbooks/*.yaml

# 🧪 Dry-run check mode
ansible-playbook playbooks/basic_setup.yaml --check

# 📋 Inspect available tasks and tags
ansible-playbook playbooks/basic_setup.yaml --list-tasks
ansible-playbook playbooks/basic_setup.yaml --list-tags
```

---

## 🤝 Contributing

This is a personal automation framework, but contributions, issues, and feature suggestions are always welcome! Feel free to fork the repository and open a pull request.

---

## ⚖️ License

Distributed under the **MIT License**. See [`LICENSE.txt`](LICENSE.txt) for more information.
