---
layout: post
title: "Discord-Voice-Channel-Streamer-Bot"
date: 2026-10-12 16:59:51 +0000
categories: projects
excerpt: "<p align='center'> <img src='assets/banner.png' alt='Discord Voice Streamer — listen to Discord voic..."
---

<p align="center">
  <img src="assets/banner.png" alt="Discord Voice Streamer — listen to Discord voice channels live in your browser" width="680" />
</p>

<p align="center">
  <img src="assets/logo.png" alt="Discord Voice Streamer logo" width="120" />
</p>

# Bridging the Gap: Streaming Discord Voice Channels to the Web

I have developed **Discord Voice Streamer**, a free, open-source, self-hosted bot designed to break the barrier between Discord voice channels and the open web. The core purpose of this project is simple: to allow anyone to listen to a live Discord voice stream directly in their browser, without requiring a Discord account.

Whether you are hosting a live podcast, broadcasting a gaming session, or simply want to make a community conversation accessible to a wider audience, this tool provides a seamless way to broadcast audio from a private server to a public (or semi-private) web link.

## ✨ Key Features

I built this project to be lightweight yet powerful, focusing on ease of access and administrative control.

### 🔊 Live Web Streaming
Once the bot joins a voice channel, it streams the audio live to a dedicated web page. I designed this to be frictionless—you simply share the link, and your listeners can tune in instantly from any modern browser.

### 🗣️ Interactive Text-to-Speech (TTS)
Interaction isn't just one-way. I've integrated Text-to-Speech capabilities, allowing you to communicate back into the Discord channel. You can trigger this in two ways:
- Using the `/tts` slash command directly within Discord.
- Typing a message through the secure web-based admin panel.

### 🖥️ Secure Admin Dashboard
To manage the bot effectively, I created a centralized admin panel. This dashboard is gated behind Discord OAuth, ensuring that only authorized users (Administrators or those with "Manage Server" permissions) can control the bot's behavior. From here, I can:
- Join or leave specific voice channels.
- Manage per-server configurations.
- Control the live stream.

### ⚡ Modern Bot Integration
The bot utilizes Discord's slash commands (`/join`, `/leave`, `/tts`) for a native user experience. Furthermore, I ensured that the bot does not require any privileged Discord intents, making it easier to deploy and more privacy-friendly.

## 📸 Interface Preview

![Admin panel](assets/screenshot-admin.png)
*The secure administrative control center.*

![Live listening page](assets/screenshot-listen.png)
*The clean, minimal interface for listeners.*

## 🚀 Potential Use Cases

There are numerous ways I envision this being used across different community types:

*   **Live Broadcasting:** Turning a Discord stage or voice channel into a live radio station for a website.
*   **Community Events:** Letting people "drop in" to listen to a Q&A session or a live panel without forcing them to create a Discord account.
*   **Accessibility:** Providing a simplified web interface for people who find the Discord client overwhelming.
*   **Remote Monitoring:** Keeping an ear on a specific channel via a browser tab while working in other applications.

## 🛠️ Technical Implementation

I developed this project using **Node.js 22+** and ensured it is **Docker-ready** for easy deployment. For those who prefer not to manage dependencies manually, the Docker image comes pre-packaged with everything needed, including `ffmpeg` for handling the TTS audio processing.

The system is designed for stability; server settings are persisted in a `server_configs.json` file, meaning the bot remembers its configuration even after a restart or redeploy.

<p>
  <img src="assets/discord.svg" alt="Discord" width="28" />
  <img src="assets/docker.svg" alt="Docker" width="28" />
  <img src="assets/nodedotjs.svg" alt="Node.js" width="28" />
</p>