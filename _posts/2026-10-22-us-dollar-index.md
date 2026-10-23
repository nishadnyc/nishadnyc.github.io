---
layout: post
title: "us-dollar-index"
date: 2026-10-22 14:24:38 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker I wanted to create a system that could continuously..."
---

# Building a Serverless U.S. Dollar Index Tracker

I wanted to create a system that could continuously monitor the strength of the U.S. Dollar without the overhead of managing a dedicated server, a database, or manual data entry. To achieve this, I built the **U.S. Dollar Index Tracker**, an automated, serverless data pipeline that collects, stores, and visualizes the Nominal Broad U.S. Dollar Index.

## What is the U.S. Dollar Index Tracker?

The U.S. Dollar Index Tracker is a specialized tool designed to track the `DTWEXBGS` series from the Federal Reserve Economic Data (FRED) API. This specific index provides a broad, trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners. 

It is important to note that this project tracks the Nominal Broad index from the Board of Governors of the Federal Reserve System, rather than the ICE U.S. Dollar Index (DXY) futures contract.

## The Purpose and Philosophy

My primary goal was to leverage GitHub as more than just a code repository. I wanted to turn it into a fully functional execution environment and storage layer. By combining GitHub Actions with a JSON-based data store, I have created a system that is:

*   **Self-Running:** Once configured, it requires zero human intervention.
*   **Serverless:** There is no backend to maintain and no hosting fees for a database.
*   **Versioned:** Because the data is stored in `prices.json` within the repo, every data update is tracked via Git commits.

## Key Features

### Automated Data Pipeline
The heart of the project is a scheduled workflow that handles the entire data lifecycle:
1. **Fetching:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
2. **Validation:** I've implemented logic to ignore invalid or unavailable observations to ensure data integrity.
3. **Storage:** New observations are appended to `prices.json`. I've included duplicate-date protection and logic to carry forward rates when FRED has no new observation for a specific calendar day.
4. **Commit:** The system automatically commits the updated JSON file back to the repository.

### High-Performance Dashboard
To make the data accessible, I built a lightweight web dashboard (`index.html`) that requires no frontend build step. It features:
*   **Modern UI:** A dark glassmorphism design with an animated count-up hero section.
*   **Instant Metrics:** A current index value display featuring a daily change pill, 30-day change, and percentage shifts.
*   **Historical Context:** Tracking for all-time highs and lows (including dates) and a counter for total days tracked.
*   **Interactive Visualizations:** Powered by Chart.js, the dashboard includes a range selector allowing users to toggle between 1 Month, 3 Months, 6 Months, 1 Year, or All-time views.
*   **Detailed Logs:** A recent records table with per-day trend tags for granular analysis.

## Potential Use Cases

This architecture provides a blueprint for several types of projects:

*   **Economic Monitoring:** For analysts who need a lightweight way to track specific FRED series over time without paying for enterprise data tools.
*   **Portfolio Tracking:** For investors who want to visualize the correlation between the U.S. Dollar's strength and other assets.
*   **Developer Education:** As a demonstration of how to use GitHub Actions as a "cron job" to create serverless data scrapers and static site generators.

By treating the repository as both the engine and the database, I've eliminated the complexity of traditional infrastructure while maintaining a high-quality, interactive data visualization tool.