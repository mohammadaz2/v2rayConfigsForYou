<div align="center">
  <h1>🚀 v2rayConfigsForYou</h1>
  <p><b>Automated Telegram V2Ray Config Scraper & Validated Subscription Links</b></p>
  
  [![Update Configs](https://github.com/mohammadaz2/v2rayConfigsForYou/actions/workflows/update_configs.yaml/badge.svg)](https://github.com/mohammadaz2/v2rayConfigsForYou/actions)
  [![Alive configs](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/mohammadaz2/v2rayConfigsForYou/main/stats.json)](configs.txt)
  [![GitHub last commit](https://img.shields.io/github/last-commit/mohammadaz2/v2rayConfigsForYou)](https://github.com/mohammadaz2/v2rayConfigsForYou/commits/main)
  [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
</div>

---

## 💥 Overview

**v2rayConfigsForYou** is a personal project turned public to help users maintain access to unrestricted internet. This repository contains an automated Python script that continuously searches for various V2Ray configurations (Vmess, Vless, Trojan, Shadowsocks) across specific Telegram channels and chats.

The configs are automatically extracted, deduplicated, **validated for reachability**, and saved every **1 hour** using GitHub Actions, ensuring you always have a fresh pool of working proxies!

## 💡 Subscription Links (How to Use)

You can easily use this repository as a subscription link in your favorite proxy client (like v2rayN, v2rayNG, NekoBox, Hiddify, Shadowrocket, etc.).

Just copy a raw URL below and paste it into your app's subscription settings:

> **All-in-one subscription (every protocol):**
> ```text
> https://raw.githubusercontent.com/mohammadaz2/v2rayConfigsForYou/main/configs.txt
> ```

> **Per-protocol subscriptions** (some clients handle these better):
> ```text
> https://raw.githubusercontent.com/mohammadaz2/v2rayConfigsForYou/main/sub/vless.txt
> https://raw.githubusercontent.com/mohammadaz2/v2rayConfigsForYou/main/sub/vmess.txt
> https://raw.githubusercontent.com/mohammadaz2/v2rayConfigsForYou/main/sub/trojan.txt
> https://raw.githubusercontent.com/mohammadaz2/v2rayConfigsForYou/main/sub/ss.txt
> ```

*To get the latest working configs, just click **Update Subscription** (Update) inside your client app!*

## ⚙️ How it Works

1. **Telegram Scraper (`finder.py`)**: Uses the `pyrogram` library (via the maintained [kurigram](https://github.com/KurimuzonAkuma/pyrogram) fork) to log into a Telegram account and read the latest messages from specified chats.
2. **Regex Parsing**: Identifies and extracts valid `vmess://`, `vless://`, `ss://`, and `trojan://` links while stripping out unwanted emojis and metadata.
3. **Deduplication**: Parsed configs are fingerprinted by protocol, host, port and credentials so duplicates are dropped.
4. **Validation**: Every config's server is checked with a **TCP-connect liveness test** (50 concurrent checks, 5s timeout) — dead servers never make it into `configs.txt`.
5. **GitHub Actions Workflow**: Runs automatically every hour via a cron job.
6. **Encrypted Sessions**: The repository uses an AES-256 encrypted session file (`my_accountb.session.aes256`) along with GitHub Secrets (`TELEGRAM_SESSION_KEY`) to safely authenticate the Telegram account without exposing sensitive data in the public repo.

## 🛠️ Setup Your Own (Forking)

If you'd like to use this code to scrape your own Telegram channels:

1. **Fork this repository.**
2. **Create your Telegram Session:**
   - Run a pyrogram script locally to generate a `.session` file for your Telegram account.
   - **Tip:** use a dedicated burner account for scraping, not your personal one.
   - Encrypt the session file using OpenSSL:
     ```bash
     openssl enc -aes-256-cbc -salt -md md5 -in your_session.session -out my_accountb.session.aes256 -pass pass:YOUR_SECRET_PASSWORD
     ```
3. **Upload the Encrypted Session:**
   - Replace the existing `my_accountb.session.aes256` in your fork with your newly encrypted file.
4. **Configure GitHub Secrets:**
   - Go to your repository **Settings > Secrets and variables > Actions**.
   - Create a new repository secret named `TELEGRAM_SESSION_KEY` and set its value to the password you used during encryption.
5. **Install dependencies locally (optional, for testing):**
   ```bash
   pip install -r requirements.txt
   ```
6. **Customize Channels (Optional)**:
   - Edit `finder.py` to target the specific Telegram chats you want to scrape.
7. **Enable Workflows**:
   - Go to the **Actions** tab in your repo and enable the workflow.

## 📁 Repository Structure

| File | Description |
| --- | --- |
| `configs.txt` | All-in-one validated subscription (all protocols) |
| `sub/vless.txt` | VLESS-only subscription |
| `sub/vmess.txt` | VMess-only subscription |
| `sub/trojan.txt` | Trojan-only subscription |
| `sub/ss.txt` | Shadowsocks-only subscription |
| `stats.json` | Live config count, used by the badge at the top |
| `finder.py` | The Telegram scraper + validator |
| `.github/workflows/update_configs.yaml` | Hourly automation |

## ⚠️ Disclaimer

This project was initially created for personal use and is shared publicly in the hopes that it will be useful. Please use the configurations responsibly and ensure you comply with all local regulations and platform terms of service.

## 📄 License

Released under the [MIT License](LICENSE).
