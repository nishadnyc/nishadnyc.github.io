---
layout: post
title: "us-dollar-index"
date: 2026-10-11 17:39:41 +0000
categories: projects
excerpt: "Building a Serverless Intraday Tracker for the US Dollar Index (DXY) Monitoring the ICE U.S. Dollar..."
---

# Building a Serverless Intraday Tracker for the US Dollar Index (DXY)

Monitoring the ICE U.S. Dollar Index (DXY) is essential for understanding the strength of the dollar against a basket of six major currencies. However, maintaining a historical database of intraday movements usually requires a paid API, a dedicated server, and a complex database setup. 

I wanted a way to track this data automatically and for free. To achieve this, I built the **DXY Intraday Tracker**, a serverless data pipeline that leverages GitHub Actions to collect, store, and visualize DXY price action without any recurring costs or manual intervention.

## The Architecture: Serverless Data Collection

The core philosophy of this project is to eliminate infrastructure overhead. Instead of a traditional backend, I use a combination of Node.js and GitHub's automation tools.

### How the Pipeline Works
1. **Scheduled Automation:** I utilize GitHub Actions to trigger a data fetch on a strict schedule: every two hours on weekdays and once daily on weekends (UTC cron).
2. **Free Data Sourcing:** The system uses `fetch-dollar.js` to request 15-minute bars for the `DX-Y.NYB` symbol directly from the Yahoo Finance chart API. This allows me to gather high-granularity data without needing an API key or a paid subscription.
3. **Intelligent Storage:** New observations are appended to a `prices.json` file. I implemented automatic backfilling, meaning if a run is delayed or skipped, the system fetches the last five days of bars to ensure no gaps exist in the history.
4. **Data Integrity:** The pipeline automatically skips duplicate timestamps and ensures `prices.json` remains chronologically sorted. On weekends, when markets are closed, a single daily snapshot carries the last close forward to maintain a consistent record.
5. **Git as a Database:** Once the data is fetched and cleaned, the GitHub Action commits the updated JSON file directly back into the repository, treating the version control system as a lightweight historical database.

## The Dashboard: Visualizing the Dollar

Collecting data is only half the battle; visualizing it is where the insights happen. I developed a dark, glassmorphism-style dashboard (`index.html`) that loads the `prices.json` file and renders it using Chart.js.

### Key Dashboard Features
*   **Interactive Visualizations:** The chart supports multiple time ranges—Day, 5D, 1M, and All. It dynamically adjusts from showing detailed intraday bars to zoomed-out daily closes.
*   **Real-time Metrics:** The hero section features an animated count-up display of the current DXY value, alongside the day's change relative to the previous close.
*   **Detailed Analytics:** I included a breakdown of the day's high and low points (with timestamps), the previous close, and a counter of total bars recorded for the day.
*   **Trend Tracking:** A recent records table provides a granular look at the latest price movements, complete with per-bar trend tags.
*   **Zero Build Step:** The frontend is pure HTML/JS, meaning it requires no build process or compilation to deploy.

## Potential Use Cases

This architecture can be applied to various financial tracking needs:

*   **Personal Trading Journals:** Keeping a permanent, versioned record of an asset's intraday movement for backtesting.
*   **Macroeconomic Monitoring:** Tracking the DXY as a lead indicator for other assets like gold or equities.
*   **Lightweight Portfolio Tracking:** Creating similar trackers for other symbols available on Yahoo Finance without paying for expensive data feeds.

## Project Structure

The project is kept lean and modular:

*   `.github/workflows/intraday-fetch.yml`: The engine that handles the scheduling.
*   `fetch-dollar.js`: The logic for API requests and data cleaning.
*   `index.html`: The glassmorphism dashboard.
*   `prices.json`: The versioned historical record of prices.

By combining the power of GitHub Actions with a simple JSON-based storage system, I've created a fully automated financial tracker that is entirely free to run and easy to maintain.