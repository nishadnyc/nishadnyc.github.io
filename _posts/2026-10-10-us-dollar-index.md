---
layout: post
title: "us-dollar-index"
date: 2026-10-10 17:16:15 +0000
categories: projects
excerpt: "Building a Serverless Intraday Tracker for the US Dollar Index (DXY) Tracking financial indices oft..."
---

# Building a Serverless Intraday Tracker for the US Dollar Index (DXY)

Tracking financial indices often feels like a choice between expensive paid APIs or the manual labor of exporting CSVs. I wanted a way to maintain a versioned, high-resolution history of the ICE U.S. Dollar Index (DXY) without paying for a subscription or managing a dedicated server. 

To solve this, I built the **DXY Intraday Tracker**, an automated, serverless data pipeline that leverages GitHub Actions as both the scheduler and the database.

## What is the DXY Intraday Tracker?

The DXY Intraday Tracker is a lightweight system designed to collect 15-minute price bars for the US Dollar Index (`DX-Y.NYB`) throughout the trading day. Instead of using a traditional database, I use a JSON file stored directly in a GitHub repository. 

By combining Node.js for data fetching and GitHub Actions for automation, I've created a "set it and forget it" pipeline that handles data collection, storage, and visualization entirely within the GitHub ecosystem.

## How the Pipeline Works

The core of the project is a serverless loop that ensures data integrity without human intervention:

1.  **Scheduled Execution:** I configured GitHub Actions to run on a specific cron schedule—every 2 hours on weekdays and once daily on weekends.
2.  **Free Data Acquisition:** The system uses `fetch-dollar.js` to request the last five days of 15-minute bars from Yahoo Finance. This approach is completely free and requires no API keys or sign-ups.
3.  **Smart Append & Backfill:** To prevent gaps in the data (which can happen if a GitHub Action run is delayed), the script fetches a window of data and appends only the records newer than the last saved entry. It automatically skips duplicate timestamps to keep `prices.json` chronologically sorted.
4.  **Weekend Handling:** Since markets are closed on weekends, the system takes a single daily snapshot to carry the last closing price forward, maintaining a consistent record per calendar day.
5.  **Versioned Storage:** Once new data is collected, the GitHub Action commits the updated `prices.json` back to the repository, creating a permanent, versioned history of the dollar's movement.

## Key Features

### Automated Data Management
*   **Zero-Cost Infrastructure:** No paid APIs and no hosting fees.
*   **Automatic Backfilling:** Built-in protection against skipped runs ensures no gaps in the historical timeline.
*   **Serverless Architecture:** The entire backend lives in `.github/workflows/`, meaning there is no server to maintain or patch.

### The Visualization Dashboard
I developed a frontend (`index.html`) that transforms the raw JSON data into a professional-grade financial dashboard. Key highlights include:
*   **Glassmorphism Design:** A modern, dark-themed interface with an animated count-up hero section.
*   **Real-time Metrics:** At a glance, I can see the current DXY price, the day's change versus the previous close, and the day's high/low with precise timestamps.
*   **Interactive Charting:** Using Chart.js, the dashboard offers multiple time-range views (Day, 5D, 1M, and All). The chart intelligently switches between intraday bars and daily closes depending on the zoom level.
*   **Trend Analysis:** A recent records table provides a granular look at the latest price action with per-bar trend tags.

## Potential Use Cases

This architecture isn't just for the DXY; it serves as a blueprint for any financial tracking project where high-frequency (but not tick-by-tick) data is needed.

*   **Personal Trading Journals:** Keep a permanent record of index levels to correlate with trade entries.
*   **Macroeconomic Monitoring:** Track the strength of the USD as a leading indicator for other asset classes like Gold or Equities.
*   **Educational Tools:** A great example of how to build a "database-less" application using Git as a versioned data store.

## Project Structure

For those interested in the technical layout, here is how the project is organized:

*   `.github/workflows/intraday-fetch.yml`: The engine that schedules and triggers the data collection.
*   `fetch-dollar.js`: The logic for fetching data from Yahoo Finance and updating the local JSON.
*   `index.html`: The frontend dashboard that renders the data.
*   `prices.json`: The "database" containing historical records in ISO 8601 format.