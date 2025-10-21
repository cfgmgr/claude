# Anthropic Claude Code — Install & Configure (via curl)

This README explains how to install a global Claude/Anthropic CLI or SDK and fetch configuration from the `cfgmgr/claude` GitHub repository using `curl`. All global installs/updates use the explicit path `/usr/bin/npm i -g -U` as requested.

> Note: This guide assumes you’ll install an npm package such as `anthropic` (official SDK) or a CLI like `claude`/`claude-cli` if you use one. Adjust names/paths to match your actual package and binary.

---

## Table of Contents

- [Quick Start](#quick-start)
- [Prerequisites](#prerequisites)
- [Install](#install)
- [Configuration](#configuration)
- [Environment Variables](#environment-variables)
- [Usage](#usage)
- [Update](#update)
- [Troubleshooting](#troubleshooting)
- [Uninstall](#uninstall)
- [Security Notes](#security-notes)
- [FAQ](#faq)
- [Support](#support)
- [License](#license)
- [Summary](#summary)

---

## Quick Start

```bash
# 1) Verify prerequisites
node -v
curl --version

# 3) Install/upgrade globally (uses the full npm path)
sudo /usr/bin/npm i -g -U "@anthropic-ai/claude-code"

# 4) Create a config directory
mkdir -p "$HOME/.claude"

# 5) Pull config from GitHub (adjust file path if needed)
CONFIG_URL="https://raw.githubusercontent.com/cfgmgr/claude/main/config.json"
curl -fsSL "$CONFIG_URL" -o "$HOME/.claude/config.json"

# 6) Set your API key
export ANTHROPIC_API_KEY=""

# 7) Verify (replace 'claude' with your CLI name if different)
claude --version || claude --help 
```

---

## Prerequisites

- **Node.js**: Version 18+ recommended.
- **npm**: Comes with Node.js. This guide uses `/usr/bin/npm` explicitly.
- **curl**: Required for fetching configuration from GitHub.
- A Unix-like shell (macOS/Linux). Windows users can use WSL or PowerShell and adapt paths.

Install Node quickly:
- macOS: `brew install node`
- Ubuntu/Debian: `sudo apt-get update && sudo apt-get install -y nodejs npm`  
  For newer Node versions, consider NodeSource, Volta, or nvm.

---

## Install

1) Confirm where `npm` lives:
```bash
which npm
ls -l /usr/bin/npm || echo "/usr/bin/npm not found; adjust commands accordingly"
```

2) Install or upgrade the global package:
```bash
# Exact path and flags as requested
sudo /usr/bin/npm i -g -U @anthropic-ai/claude-code
```

3) Confirm the CLI/library is available:
```bash
which claude || echo "If no 'claude' on PATH, check your CLI package name or global npm bin path."
claude --help || true
```

> If you are using the SDK only (`anthropic`), there may be no CLI binary. Use it programmatically in Node.js.

---

## Configuration

This guide stores configuration under `~/.claude`. It pulls a JSON config file from the `cfgmgr/claude` repository using GitHub’s raw content endpoint.

1) Create the config directory:
```bash
CONFIG_DIR="$HOME/.claude"
mkdir -p "$CONFIG_DIR"
```

2) Choose the raw config URL. Update the branch/path/file as needed:
```bash
CONFIG_URL="https://raw.githubusercontent.com/cfgmgr/claude/main/config.json"
```

3) Download the config:
```bash
curl -fsSL "$CONFIG_URL" -o "$CONFIG_DIR/config.json" && echo "Wrote config to: $CONFIG_DIR/config.json"
```

4) Sanity-check the config content:
```bash
head -n 20 "$CONFIG_DIR/config.json" || true
```

## Environment Variables

Most Claude-based tools require an API key.

- Set your Anthropic API key:
```bash
export ANTHROPIC_API_KEY=""
```

- Optional variables (adjust per your tool’s docs):
  - `CLAUDE_CONFIG_PATH`: Absolute path to a specific config file
  - `ANTHROPIC_API_URL`: Override base URL if using a proxy/gateway
  - `HTTP_PROXY` / `HTTPS_PROXY`: Proxy support if required
  - `NO_PROXY`: Comma-separated list of hosts not to proxy
  - `DEBUG=1`: Enable verbose logging if supported

Persist environment variables by adding them to your shell profile (e.g., `~/.bashrc`, `~/.zshrc`).

---

## Usage

- CLI usage (if you installed a `claude` CLI):
```bash
claude --help
claude run --config "$HOME/.claude/config.json"
```

## Update

- Upgrade the global package to the latest version:
```bash
sudo /usr/bin/npm i -g -U @anthropic-ai/claude-code
```

- Refresh your configuration from GitHub:
```bash
CONFIG_URL="https://raw.githubusercontent.com/cfgmgr/claude/main/config.json"
curl -fsSL "$CONFIG_URL" -o "$HOME/.claude/config.json"
```

---

## Troubleshooting

- “/usr/bin/npm: No such file or directory”
  - Your system’s `npm` may be elsewhere (e.g., `/usr/local/bin/npm`). Run `which npm` and either adjust the path or create a symlink.
- “Permission denied” during global install
  - Use `sudo`, or configure a user-level global prefix:  
    `npm config set prefix "$HOME/.npm-global"` and add `$HOME/.npm-global/bin` to your PATH.
- CLI not found after install
  - Ensure the package actually provides a binary and your global npm bin dir is on `PATH`. Check `npm bin -g`.
- 404 or error when fetching config
  - Verify the repo, branch (`main` vs `master`), file path, and that your network/proxy allows GitHub access.
- Proxy issues
  - Set `HTTP_PROXY`, `HTTPS_PROXY`, and possibly `NO_PROXY`. Test with `curl -v`.
- Config format mismatch
  - Confirm your tool expects JSON vs YAML/TOML and point `--config` or env vars to the correct file.

---

## Uninstall

```bash
sudo /usr/bin/npm uninstall -g anthropic
rm -Rf "$HOME/.claude" 2>/dev/null || true
```

---

## Security Notes

- Never commit secrets (like `ANTHROPIC_API_KEY`) to version control.
- Restrict permissions on your config directory if it contains sensitive values:
```bash
chmod 700 "$HOME/.claude"
chmod 600 "$HOME/.claude/config.json"
```
- Consider using a secrets manager or environment variable injection for CI/CD.

---

## FAQ

- “Can I use a different config location?”
  - Yes. Either set `CLAUDE_CONFIG_PATH` (if supported) or pass a `--config` flag to the CLI if it provides one.

---

## Support

- Issues with the npm package: open an issue in that package’s repository.
- Problems with configuration retrieval: verify the file path and branch in `cfgmgr/claude`, then open an issue there if needed.

---

## License

Include your project’s license information here (e.g., MIT). If you’re consuming third-party packages, follow their licenses accordingly.

---

## Summary

- Installed the global package using `sudo /usr/bin/npm i -g -U @anthropic-ai/claude-code`
- Fetched configuration from `cfgmgr/claude` with `curl` and stored it under `~/.claude/config.json`.
- Set environment variables, verified the CLI/library, and provided steps to update, troubleshoot, and uninstall.

## Author  

⛵ cfgmgr: [Github](https://github.com/cfgmgr) ⛵  
🤖 casjay: [Github](https://github.com/casjay) 🤖  


