---
layout: post
title: "us-dollar-index"
date: 2026-10-09 22:45:37 +0000
categories: projects
excerpt: "Tracking the U.S. Dollar Index: A Serverless Intraday Pipeline Monitoring the ICE U.S. Dollar Index..."
---

# Tracking the U.S. Dollar Index: A Serverless Intraday Pipeline

Monitoring the ICE U.S. Dollar Index (DXY) is essential for understanding the strength of the USD against a basket of major global currencies. However, getting high-frequency intraday data often requires expensive API subscriptions or managing a dedicated server to poll for updates.

I built the **DXY Intraday Tracker** to solve this problem. It is a fully automated, serverless data pipeline that collects DXY price action throughout the trading day and stores it as versioned history—all without a paid API or a dedicated backend.

## How the System Works

My goal was to create a "set it and forget it" system. I leveraged GitHub Actions to handle the orchestration, turning the repository itself into both the database and the execution engine.

The pipeline operates in a continuous loop:

1.  **Scheduled Execution:** GitHub Actions triggers a workflow every two hours on weekdays and once daily on weekends.
2.  **Data Acquisition:** I use a script called `fetch-dollar.js` to request the last five days of 15-minute bars for the symbol `DX-Y.NYB` from the Yahoo Finance chart API. This allows me to gather data for free without needing an API key.
3.  **Smart Storage:** The system compares the fetched bars against the existing `prices.json` file. Any record newer than the last saved entry is appended. I've implemented automatic backfilling, meaning that if a run is delayed or skipped, the system fills the gaps on the next successful execution.
4.  **Persistence:** Once the new data is appended and duplicates are filtered out, the GitHub Action commits the updated `prices.json` back to the repository.
5.  **Visualization:** The `index.html` dashboard reads this JSON file and renders the data using Chart.js.

## Key Features

I designed the project to be lightweight yet visually impactful. Here are the standout features:

### 🛠️ Serverless Automation
By using GitHub Actions, I eliminated the need for a VPS or cloud hosting. The data collection happens in the cloud, and the storage is handled by Git versioning.

### 📊 Glassmorphism Dashboard
The frontend is a modern, dark-themed dashboard utilizing glassmorphism effects. It provides an immediate snapshot of the market, featuring:
*   **Animated Hero Section:** A count-up display of the current DXY value.
*   **Market Metrics:** Real-time day change versus the previous close, day high/low with timestamps, and a counter for the number of bars recorded today.
*   **Interactive Charts:** Powered by Chart.js, users can toggle between Day, 5-Day, 1-Month, and "All" time ranges. The chart intelligently switches from intraday bars to daily closes as the time range expands.
*   **Trend Analysis:** A recent records table that includes per-bar trend tags to quickly identify price movement.

### 🛡️ Data Integrity
To ensure the dataset remains clean, I built in duplicate protection and chronological sorting. On weekends, when markets are closed, the system carries the last close forward to maintain a consistent daily record.

## Potential Use Cases

This architecture provides a blueprint for anyone looking to track financial instruments without incurring costs. Some potential applications include:

*   **Personal Trading Journals:** Automatically logging the DXY to correlate dollar strength with other asset classes (like Gold or BTC).
*   **Lightweight Market Monitoring:** A quick-glance dashboard for traders who don't want to open a heavy trading platform.
*   **Educational Data Sets:** A clean, versioned JSON history of the DXY that can be used for backtesting simple strategies or practicing data analysis.

## Project Architecture

The project is structured for simplicity and transparency:

*   `.github/workflows/intraday-fetch.yml`: The heartbeat of the project (the cron schedule).
*   `fetch-dollar.js`: The logic for API requests and data filtering.
*   `prices.json`: The historical database storing timestamps and prices.
*   `index.html`: The frontend visualization layer.

By combining the power of GitHub Actions with a free public API, I've created a sustainable, zero-cost way to maintain a high-fidelity history of the U.S. Dollar Index.