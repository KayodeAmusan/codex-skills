# Codex Skills Collection

Backup and sharing repository for custom Codex skills.

This repository contains 395 user-level skills collected from Windows and Linux Codex installations:

```text
Windows: %USERPROFILE%\.codex\skills
Linux:   ~/.codex/skills
```

Codex's built-in `.system` skills and plugin cache files are intentionally not included. Install Codex and the required plugins normally on each computer.

## Install on Windows

Clone the repository, then copy the skill directories into your user-level Codex skills directory:

```powershell
git clone https://github.com/KayodeAmusan/codex-skills.git
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex\skills" | Out-Null
robocopy ".\codex-skills\skills" "$env:USERPROFILE\.codex\skills" /E
```

`robocopy` exit codes from 0 through 7 indicate success. Restart Codex or begin a new chat after copying.

## Install on macOS or Linux

```bash
git clone https://github.com/KayodeAmusan/codex-skills.git
mkdir -p "$HOME/.codex/skills"
cp -R codex-skills/skills/. "$HOME/.codex/skills/"
```

Restart Codex or begin a new chat after copying.

## Security notice

Review every skill before enabling it on another computer. This collection includes development, security-testing, browser-automation, infrastructure, and scaffolding skills. Some intentionally contain scripts that can execute commands, access the network, create or remove files, or install dependencies.

The export was scanned for hardcoded credentials before its initial publication. Scanner matches were reviewed and identified as test fixtures, documented example credentials, localhost connection strings, or replacement placeholders. This is not a guarantee that every skill is safe for every environment.

Do not add `.env` files, credentials, private keys, access tokens, or personal data to this repository.

