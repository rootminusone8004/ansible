# 🌍 Personal Ansible Repository

![License: MIT](https://img.shields.io/badge/License-MIT-800000.svg)

## 📖 Overview

This repository contains my personal [Ansible](https://www.ansible.com/) playbooks and roles for automating system configuration, application deployment, and infrastructure management.

It serves as a centralized hub for managing tasks across different environments and servers using consistent, repeatable, and version-controlled automation.

## ✅ Prerequisites

Make sure you have the following installed and configured:

- **🐍 Python 3.6+**
- **⚙️ Ansible v2.10+** — [Installation Guide](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- 🔑 SSH access to target hosts
- 🖥️ Inventory file with managed hosts

## 🚀 Getting Started

### 1. Clone the Repository

``` bash
$ git clone 'https://github.com/rootminusone8004/ansible'
$ cd ansible
```

### 2. Set Up Inventory

Make change in the [hosts.yaml](inventory/hosts.yaml) file. It is the default one. But you can set others as well.

``` yaml
all:
  hosts:
    52.71.148.104:
      ansible_user: admin
```

### 3. Run a playbook

``` bash
ansible-playbook playbooks/basic_setup.yaml
```

Add `--ask-become-pass` if your tasks require `sudo`.

## 🗂️ Repository Structure

``` bash
.
├── ansible.cfg
├── inventory
│   ├── hosts.yaml
│   ├── kubeadm.yaml
│   ├──   ...
|   
├── playbooks
│   ├── basic_setup.yaml
│   ├── docker.yaml
│   ├──   ...
│
├── tasks
│   ├── docker.yaml
│   ├── dotfiles.yaml
│   ├──   ...
│
├── README.md
└── LICENSE.txt
```

## 📚 Best Practices

- 🧪 Test playbooks using tools like Vagrant
- 🔐 Store secrets securely using [Ansible Vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html)
- 🧱 Modularize playbooks using roles for clarity and reuse
- ✅ Use `ansible-lint` to validate your code


## 🤝 Contributing

This is a personal automation toolkit, but feel free to fork or open issues for feedback, suggestions, or improvements.

## ⚖️ License

This project is licensed under **MIT license**. See the [LICENSE](LICENSE.txt) file for details.
