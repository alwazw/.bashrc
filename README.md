---

# 🚀 Pro-Spec Ubuntu Infrastructure Bootstrapper


A streamlined, declarative, and idempotent system initialization environment. This repository moves away from passive configuration files and serves as an automated Infrastructure-as-Code (IaC) baseline that transforms a bare Ubuntu server into a hardened, Docker-ready development environment in under 60 seconds.

---



## 🛠️ System Architecture & Logic Flow

Unlike traditional dotfile repositories that naively append blocks of text—causing configuration bloat and duplicates—this system uses a **Single Stitch Include Pattern**.

1. **Upstream Linkage:** The installation script establishes a symbolic link from your local `$HOME/.bashrc` directly to the version-controlled `.bashrc` inside this repository directory (`~/.bashrc_repo`).
2. **Clean State Aliasing:** Instead of mutating the version-controlled `.bashrc` directly, the bootstrap runner generates a completely isolated local file at `~/.bash_aliases_pro`. This file handles all extracted performance, networking, and system maintenance aliases.
3. **Idempotent Insertion:** The script scans your `.bashrc` for exactly *one* declaration: `[ -f ~/.bash_aliases_pro ] && source ~/.bash_aliases_pro`. If missing, it links it once. If present, it skips it—ensuring zero terminal configuration rot over multiple runs.



```
  +-----------------------------------------+
  |        🚀 Target Linux Engine           |
  +-----------------------------------------+
                       |
                       v
          Executes: ./init_pro.sh
                       |
   +-------------------+-------------------+
   |                                       |
   v                                       v

```

[ Provisioning Phase ]                [ Configuration Handshake ]

* Package Indexes Updated              - Clones/Pulls this Git Repo
* Official Repos Registered            - Symlinks ~/.bashrc -> Repo
* Core Tool Suite Installed            - Generates Managed Local File
* Firewall & zRAM Enabled                (~/.bash_aliases_pro)
- Adds single "Stitch Line"
to prevent duplicate logs

```

---

## 📦 What gets Provisioned?

The runner provisions a performance-optimized tool suite aggregated from production engineering requirements:

* **Containerization:** Official Docker Engine (`docker-ce`), CLI tools, and the Modern Compose V2 Plugin (bypassing outdated upstream mirror packages).
* **Performance Monitors:** `btop` (high-res metrics), `htop`, and `ncdu` (interactive disk analyzer).
* **Utilities:** `fzf` (fuzzy finder), `bat` (syntax-highlighted cat), `ripgrep`, `jq`, `tree`, and `build-essential`.
* **State Control:** `etckeeper` automatic tracking for system configurations inside `/etc`.
* **Resource Optimization:** `zram-config` to enable memory compression automatically.
* **Security & Hardening:** `ufw` firewall configured to block everything except core ports (`22`, `80`, `443`), `fail2ban` bruteforce protection, and automated execution of `qemu-guest-agent`.

---

## 🚀 Quickstart: Provisioning a New Machine

To deploy your entire stack on a brand-new, bare Ubuntu instance, execute this single workflow:

```bash
# 1. Clone your configuration matrix down
git clone [https://github.com/alwazw/.bashrc.git](https://github.com/alwazw/.bashrc.git) ~/.bashrc_repo

# 2. Change into the workspace
cd ~/.bashrc_repo

# 3. Mark the bootstrapper executable
chmod +x init_pro.sh

# 4. Fire the script
./init_pro.sh

```

> ⚠️ **Security Architecture Note:** This configuration intentionally abandons the dangerous passwordless `NOPASSWD:ALL` sudo rules. The script will securely request your administrative password *once* at launch, cache it safely for the loop execution, and complete the installation without compromising machine authority.

---

## 🛠️ Post-Installation Actions

Once the execution log reports a clean, green exit status:

1. **Reload your active environment profile:**
```bash
source ~/.bashrc

```


2. **Apply Security Group Mutations:** Log out and back into the machine to finalize your user's administrative group mapping into the `docker` subsystem.

---

## 📂 Repository File Blueprint

```text
.bashrc_repo/
├── README.md       # Architectural map & user manual
├── .bashrc         # Primary dotfile linked to your system environment
└── init_pro.sh     # Master automated provisioning engine

```

## 💡 Suggestions to Make Changes to your Repo

Since you are treating your repo as a clean starting point, here are three high-value structural changes you should make inside your GitHub repository right now to maximize this setup:

### 1. Structure the Repo File Hierarchy Cleanly
Make sure your online repository contains exactly these three files at the root level:
* `.bashrc`
* `init_pro.sh`
* `README.md`

Remove any old, half-working scripts (like `0. Alias_Full_Setup.sh`, `v2`, or old versions of `s.sh`) so you have a pristine workspace. 

### 2. Clean Up your Repository's `.bashrc`
Since `init_pro.sh` dynamically builds and overwrites your custom local alias paths every time it runs, your `.bashrc` inside the GitHub repo should be kept completely clean and lightweight. It only needs to contain environmental exports and the single line to load your managed aliases. 

Open the `.bashrc` file in your repository and make sure it looks like this:
```bash
# ==============================================================================
# 🌌 MAIN GIT-MANAGED PROFILE (.bashrc)
# ==============================================================================

# Global paths for custom local application stacks
export PATH="$HOME/.npm-global/bin:$PATH"
export PATH="/usr/bin:$PATH"
export PATH="$HOME/.browseros/bin:$PATH"

# Managed Pro-Spec Internal Aliases Handshake
# (This line connects your system to the local engine generated by your init script)
[ -f ~/.bash_aliases_pro ] && source ~/.bash_aliases_pro

```

### 3. Track `.env_secrets` Safely with a Template

You have a brilliant configuration logic in your script that parses a secure local file called `~/.env_secrets` for keys or tokens without committing them to the open web.

To keep this process reproducible, create an empty template file in your repo called `.env_secrets.example`. Put this inside it:

```bash
# ==============================================================================
# 🔐 LOCAL ENVIRONMENT SECRETS TEMPLATE
# Copy this file to ~/.env_secrets on your machine and fill in your keys.
# DO NOT COMMIT THE ACTUAL SEED RESIDUE TO GITHUB!
# ==============================================================================
export GEMINI_API_KEY="your_api_key_here"
export COPILOT_TOKEN="your_token_here"

```

This lets you instantly see what keys a new machine requires without risking exposing confidential access codes to a public GitHub workspace.
