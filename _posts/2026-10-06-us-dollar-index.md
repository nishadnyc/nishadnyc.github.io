---
layout: post
title: "us-dollar-index"
date: 2026-10-06 18:59:57 +0000
categories: projects
excerpt: "Tracking the US Dollar Index: A Serverless, Automated Approach Tracking the ICE U.S. Dollar Index (..."
---

# Tracking the US Dollar Index: A Serverless, Automated Approach

Tracking the ICE U.S. Dollar Index (DXY) is essential for anyone monitoring the global economy, as it measures the strength of the US dollar against a basket of six major currencies. However, getting a consistent, historical record of intraday movements often requires expensive paid APIs or the overhead of maintaining a dedicated server.

I built the **DXY Intraday Tracker** to solve this problem. It is a fully automated, serverless data pipeline that collects DXY data throughout the trading day and stores it as versioned history—all without a single cent in infrastructure costs.

## The Architecture: Serverless Data Collection

The core philosophy behind this project is to remove the need for human intervention and paid services. I leveraged GitHub Actions to act as both the scheduler and the compute engine.

### How the Pipeline Works
1. **Scheduled Execution:** I configured GitHub Actions to run on a specific cron schedule: every two hours on weekdays and once daily on weekends (UTC).
2. **Free Data Sourcing:** I use a Node.js script (`fetch-dollar.js`) to request 15-minute bars for the symbol `DX-Y.NYB` from the Yahoo Finance chart API. This eliminates the need for API keys or sign-up processes.
3. **Intelligent Backfilling:** To ensure the data remains continuous, the script fetches the last five days of data on every run. It appends any bar newer than the last saved record, meaning that if a run is skipped or delayed, the pipeline automatically fills the gaps.
4. **Data Integrity:** The system automatically skips duplicate timestamps and keeps `prices.json` chronologically sorted. On weekends, when markets are closed, a single daily snapshot carries the last close forward to maintain a consistent record per calendar day.
5. **Versioned Storage:** Once the data is fetched and filtered, GitHub commits the updated `prices.json` back to the repository, creating a permanent, versioned history of the index.

## Key Features

### 🛠 Automated Pipeline
The entire lifecycle—from data fetching to storage—is hands-off. Once configured, the project operates independently.

### 📊 Glassmorphism Dashboard
I developed a modern, dark-themed dashboard (`index.html`) to visualize the collected data. The interface features:
* **Animated Hero Section:** A count-up animation showing the current DXY value.
* **Real-time Metrics:** Instant visibility of the day's change versus the previous close, as well as the day's high and low with corresponding timestamps.
* **Interactive Visualization:** Powered by Chart.js, the dashboard allows users to toggle between Day, 5D, 1M, and "All" ranges. The chart intelligently adjusts, showing granular intraday bars when zoomed in and daily closes when zoomed out.
* **Trend Tracking:** A recent records table includes per-bar trend tags to quickly identify momentum.

### ⚡ Zero-Build Frontend
The dashboard is designed for simplicity; it loads the stored JSON data directly and renders the UI without requiring a complex frontend build step.

## Potential Use Cases

This project is ideal for several different scenarios:

* **Personal Financial Tracking:** For traders or investors who want a private, permanent record of DXY movements without paying for a Bloomberg terminal or premium data feed.
* **Economic Research:** Because the data is stored in a Git repository, it provides an immutable audit trail of the index's movement over time.
* **Learning Serverless Patterns:** This project serves as a blueprint for how to use GitHub Actions as a "poor man's cron job" to build data scrapers and trackers.

## Project Structure

The project is lightweight and organized for clarity:

* `.github/workflows/intraday-fetch.yml`: The automation engine.
* `fetch-dollar.js`: The logic for fetching and storing new bars.
* `index.html`: The glassmorphism dashboard interface.
* `prices.json`: The historical database containing time-stamped records (ISO 8601, America/New_York).