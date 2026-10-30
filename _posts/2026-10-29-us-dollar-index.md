---
layout: post
title: "us-dollar-index"
date: 2026-10-29 15:56:23 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker Tracking global currency trends usually requires ex..."
---

# Building a Serverless U.S. Dollar Index Tracker

Tracking global currency trends usually requires expensive subscriptions or manually exporting CSV files from financial databases. I wanted a way to continuously monitor the strength of the U.S. Dollar without managing a server, paying for hosting, or performing manual updates. 

To solve this, I built the **U.S. Dollar Index Tracker**: an automated, serverless data pipeline that collects and stores the Nominal Broad U.S. Dollar Index.

## What is the U.S. Dollar Index Tracker?

The project is a self-sustaining system designed to track the `DTWEXBGS` series from the Federal Reserve Economic Data (FRED) API. Specifically, it monitors the Nominal Broad U.S. Dollar Index—a trade-weighted measure of the U.S. dollar against the currencies of its major trading partners.

The core philosophy behind this project was to leverage GitHub not just as a place to store code, but as the entire infrastructure. I use GitHub Actions as the execution engine and the repository's own file system as the versioned database.

## How the Pipeline Works

I have designed the system to run with zero human intervention. The workflow follows a precise automated cycle:

1. **Scheduled Trigger:** A GitHub Actions workflow triggers on a set schedule.
2. **Data Retrieval:** A Node.js script (`fetch-dollar.js`) requests the latest daily observation from the FRED API.
3. **Validation:** The system filters out invalid or unavailable observations to ensure data integrity.
4. **Storage:** New observations are appended to a `prices.json` file. I've implemented duplicate-date protection to ensure each calendar day has exactly one record; if FRED has no new observation for a day, the previous rate is carried forward.
5. **Persistence:** The updated JSON file is automatically committed and pushed back to the repository.
6. **Visualization:** The `index.html` dashboard pulls this JSON data to render real-time insights.

## Key Features

### Automated Infrastructure
* **Serverless Execution:** No dedicated backend or VPS is required.
* **Versioned Storage:** Historical data is stored directly in the repo, providing a permanent, auditable trail of currency movements.
* **Zero Build Step:** The frontend is lightweight and requires no compilation or build process.

### Advanced Dashboard
I wanted the data to be as visually intuitive as the collection process is efficient. The dashboard features:
* **Dark Glassmorphism UI:** A modern aesthetic with an animated count-up hero section.
* **At-a-Glance Metrics:** A current index value with a daily change pill, as well as 30-day change and percentage calculations.
* **Historical Context:** Automatic tracking of all-time highs and lows (including dates) and a counter for total days tracked.
* **Interactive Visualization:** A Chart.js integration allowing users to toggle between 1-month, 3-month, 6-month, 1-year, and "All" time ranges.
* **Detailed Records:** A recent records table featuring per-day trend tags.

## Potential Use Cases

This architecture serves as a blueprint for anyone looking to create "set-and-forget" data trackers. Potential use cases include:

* **Economic Monitoring:** For analysts who need a lightweight way to track Federal Reserve indices without navigating the FRED website daily.
* **Portfolio Correlation:** For investors wanting to see how their assets correlate with the broad strength of the USD over time.
* **Educational Proof-of-Concept:** As a demonstration of how to use GitHub Actions as a cron-job and GitHub as a flat-file database.

By combining the FRED API with GitHub's automation tools, I've created a tool that transforms raw economic data into a polished, historical archive and visual dashboard.