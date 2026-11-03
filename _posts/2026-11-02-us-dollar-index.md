---
layout: post
title: "us-dollar-index"
date: 2026-11-02 19:17:31 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker Tracking currency trends usually requires expensive..."
---

# Building a Serverless U.S. Dollar Index Tracker

Tracking currency trends usually requires expensive subscriptions or manually downloading CSV files. I wanted a way to continuously monitor the Nominal Broad U.S. Dollar Index without managing a dedicated server or performing manual updates. To achieve this, I built a fully automated, serverless data pipeline that leverages GitHub as both an execution environment and a database.

## What is the U.S. Dollar Index Tracker?

The U.S. Dollar Index Tracker is an automated system designed to collect and store the **Nominal Broad U.S. Dollar Index** (Series: `DTWEXBGS`). This specific index, provided by the Board of Governors of the Federal Reserve System, is a trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners.

It is important to note that this project specifically tracks the broad index from FRED, rather than the ICE U.S. Dollar Index futures contract (DXY).

## The Architecture: Serverless Data Collection

My primary goal was to create a self-running system. I achieved this by utilizing GitHub Actions to handle the backend logic and the repository itself to handle data storage.

### How the Pipeline Works
1. **Scheduled Execution**: A GitHub Actions workflow triggers on a set schedule.
2. **Data Fetching**: A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3. **Validation & Storage**: The system ignores invalid or unavailable observations. Valid new data is appended to a `prices.json` file.
4. **Versioned Persistence**: GitHub commits the updated `prices.json` back to the repository, creating a permanent, versioned historical record.
5. **Visualization**: A lightweight frontend (`index.html`) reads this JSON file to display the data.

By using this approach, I have eliminated the need for a dedicated backend or a traditional database, making the entire project maintenance-free.

## Key Features

### Automated Data Integrity
To ensure the dataset remains clean and usable, I implemented several safeguards:
* **Daily Continuity**: The system ensures one record per calendar day. If FRED has no new observation for a specific date, the previous rate is carried forward.
* **Duplicate Protection**: Built-in logic prevents duplicate entries for the same date.
* **Hands-off Operation**: Once configured, the pipeline requires zero human intervention.

### The Analytics Dashboard
I developed a modern, high-performance dashboard to make the collected data actionable. The interface features:
* **Glassmorphism Design**: A dark-themed UI with an animated count-up hero section.
* **Real-time Metrics**: Instant visibility of the current index value, daily change "pills," and 30-day percentage shifts.
* **Historical Context**: Automatic calculation of all-time highs and lows, including the dates they occurred and a total "days-tracked" counter.
* **Interactive Visualizations**: Powered by Chart.js, the dashboard allows users to toggle between 1-month, 3-month, 6-month, 1-year, and all-time views.
* **Detailed Records**: A recent records table with per-day trend tags for granular analysis.

## Potential Use Cases

This project serves as a blueprint for anyone looking to track economic indicators or financial time-series data. Potential applications include:

* **Economic Monitoring**: Tracking the strength of the USD to understand its impact on international trade and imports/exports.
* **Portfolio Correlation**: Analyzing how a broad dollar index correlates with other asset classes like gold or equities.
* **Educational Templates**: Serving as a reference for building serverless "Cron-job" style applications using GitHub Actions.

## Project Structure

The project is kept lightweight and modular:

* `.github/workflows/update-dollar.yml`: The heart of the automation.
* `fetch-dollar.js`: The logic for API interaction and data storage.
* `index.html`: The presentation layer and user dashboard.
* `prices.json`: The historical data store.