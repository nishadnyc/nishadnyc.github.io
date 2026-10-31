---
layout: post
title: "us-dollar-index"
date: 2026-10-30 15:41:20 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker I wanted to create a system that could continuously..."
---

# Building a Serverless U.S. Dollar Index Tracker

I wanted to create a system that could continuously monitor and store the Nominal Broad U.S. Dollar Index without the overhead of managing a dedicated server or manually updating spreadsheets. To achieve this, I built the U.S. Dollar Index Tracker—an automated, serverless data pipeline that leverages GitHub as both its execution engine and its database.

## What is the U.S. Dollar Index Tracker?

The U.S. Dollar Index Tracker is a specialized tool designed to collect and visualize the **Nominal Broad U.S. Dollar Index (Series: DTWEXBGS)**. Unlike the commonly cited DXY (ICE U.S. Dollar Index futures), this project tracks the broad, trade-weighted measure provided by the Board of Governors of the Federal Reserve System.

The core philosophy of this project was to eliminate "infrastructure friction." By using a serverless architecture, I've ensured that the data collection happens autonomously in the background, requiring zero human intervention once configured.

## How the Pipeline Works

I designed the project to treat the GitHub repository as a versioned storage layer. Here is the step-by-step flow of how the data moves from the Federal Reserve to the user's screen:

1. **Scheduled Trigger:** A GitHub Action workflow runs on a predefined schedule.
2. **Data Fetching:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3. **Validation & Storage:** The system filters out invalid or unavailable observations. If a new observation is found, it is appended to `prices.json`. To ensure data continuity, I implemented logic to carry the rate forward when FRED has no new observation for a specific day.
4. **Automated Commit:** GitHub automatically commits the updated `prices.json` file back to the repository.
5. **Visualization:** The `index.html` dashboard reads this JSON file and renders it using Chart.js.

## Key Features

### 🛠️ Automation & Infrastructure
- **Serverless Execution:** No backend server is required; the entire logic runs via GitHub Actions.
- **Versioned History:** Because data is stored in a JSON file within the repo, I have a complete, version-controlled history of the index.
- **Duplicate Protection:** I've integrated automatic duplicate-date protection to keep the dataset clean.

### 📊 The Dashboard
I wanted the presentation layer to be as polished as the backend. The dashboard features a dark glassmorphism aesthetic and includes:
- **Animated Hero Section:** A count-up animation displaying the current index value.
- **Instant Insights:** A daily change pill, 30-day change metrics, and percentage shifts.
- **Historical Extremes:** Real-time tracking of all-time highs and lows, including the specific dates they occurred.
- **Interactive Charting:** A Chart.js visualization with a range selector allowing users to toggle between 1 month, 3 months, 6 months, 1 year, or the full history.
- **Trend Analysis:** A recent records table featuring per-day trend tags.

## Potential Use Cases

This architecture provides a blueprint for anyone looking to track financial or economic indicators over time. Potential use cases include:

*   **Economic Research:** Maintaining a clean, accessible historical record of the Broad USD Index for analysis.
*   **Portfolio Monitoring:** Using the index as a benchmark to understand how currency strength is impacting international assets.
*   **Learning Serverless Patterns:** Serving as a reference for how to use GitHub Actions for data scraping and "Git-as-a-database" workflows.

By combining the FRED API, GitHub Actions, and a lightweight frontend, I have created a completely hands-off system that transforms raw economic data into actionable visual insights.