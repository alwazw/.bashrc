# 🚀 Pro-Spec Ubuntu Infrastructure Bootstrapper

A streamlined, declarative, and idempotent system initialization environment. This repository moves away from passive configuration files and serves as an automated Infrastructure-as-Code (IaC) baseline that transforms a bare Ubuntu server into a hardened, Docker-ready development environment in under 60 seconds.

---

## 🛠️ System Architecture & Logic Flow

Unlike traditional dotfile repositories that naively append blocks of text—causing configuration bloat and duplicates—this system uses a **Single Stitch Include Pattern**.

1. **Upstream Linkage:** The installation script establishes a symbolic link from your local `$HOME/.bashrc` directly to the version-controlled `.bashrc` inside this repository directory (`~/.bashrc_repo`).
2. **Clean State Aliasing:** Instead of mutating the version-controlled `.bashrc` directly, the bootstrap runner generates a completely isolated local file at `~/.bash_aliases_pro`. This file handles all extracted performance, networking, and system maintenance aliases.
3. **Idempotent Insertion:** The script scans your `.bashrc` for exactly *one* declaration: `[ -f ~/.bash_aliases_pro ] && source ~/.bash_aliases_pro`. If missing, it links it once. If present, it skips it—ensuring zero terminal configuration rot over multiple runs.
