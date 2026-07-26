---
name: deliver-via-download-link
description: "User delivery preference: when sending files (images, NFT art, banners, logos, scripts) to this user, ALWAYS share via an HTTP download link from the VPS (http://134.199.170.183:8000/ or :8002/) — NEVER send the media directly through Telegram/WhatsApp chat. Triggered by user saying 'ingat pake link download!' or any file-delivery request. This overrides the default Telegram MEDIA:/path behavior."
version: 1.0.0
author: Community
license: MIT
platforms: [linux, macos, windows]
tags: [general]
---

# Deliver Via Download Link — Skill

User delivery preference: when sending files (images, NFT art, banners, logos, scripts) to this user, ALWAYS share via an HTTP download link from the VPS (http://134.199.170.183:8000/ or :8002/) — NEVER send the media directly through Telegram/WhatsApp chat. Triggered by user saying 'ingat pake link download!' or any file-delivery request. This overrides the default Telegram MEDIA:/path behavior.

## Install

```bash
cp -r <skill-name> ~/.hermes/skills/<skill-path>/
```

Or clone this repository:

```bash
git clone https://github.com/iizcm/deliver-via-download-link-skill.git ~/.hermes/skills/<skill-path>/
```

## Usage

Invoke your AI agent with a clear instruction matching this skill's purpose. The agent will route tasks to this skill when the instruction matches its description or trigger keywords.

Refer to `README.md` in this repository for:
- Detailed step-by-step installation guide
- Bilingual documentation (English + Indonesian)
- Troubleshooting table
- Security best practices
- Customization tips

## Safety rules

- Never commit private keys, seed phrases, API tokens, or personal data to version control
- Use placeholders (`<YOUR_...>`) in all examples and code snippets
- Validate all outputs before acting on them
- Keep real credentials in your runtime's secure credential store only
