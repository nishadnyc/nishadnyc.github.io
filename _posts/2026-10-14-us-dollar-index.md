---
layout: post
title: "us-dollar-index"
date: 2026-10-14 14:24:29 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker I have developed a fully automated, serverless data..."
---

# Building a Serverless U.S. Dollar Index Tracker

I have developed a fully automated, serverless data pipeline designed to continuously collect and store the Nominal Broad U.S. Dollar Index. My goal was to create a system that maintains a precise historical record of currency strength without the overhead of a dedicated server, manual data entry, or a complex backend infrastructure.

## What is the U.S. Dollar Index Tracker?

At its core, this project is a self-sustaining data collection engine. It targets the `DTWEXBGS` series from the Federal Reserve Economic Data (FRED) API, which provides the Nominal Broad U.S. Dollar Index. Unlike the common DXY futures contract, this index is a broad, trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners.

To achieve a "zero-maintenance" architecture, I leveraged GitHub as both the execution environment and the database.

## How the Pipeline Works

I designed the system to operate in a continuous loop using GitHub Actions. Here is the technical flow of the automation:

1. **Scheduled Execution:** A GitHub Actions workflow triggers on a set schedule.
2. **Data Fetching:** I wrote a Node.js script, `fetch-dollar.js`, which requests the latest observation from the FRED API.
3. **Validation:** The system automatically filters out invalid or unavailable observations to ensure data integrity.
4. **Storage:** New observations are appended to a `prices.json` file. I implemented duplicate-date protection to ensure each calendar day has only one record; if FRED has no new observation for a specific day, the previous rate is carried forward.
5. **Versioned Commits:** Once the data is updated, the GitHub Action commits the updated `prices.json` directly back into the repository.
6. **Visualization:** A lightweight frontend (`index.html`) reads this JSON file to render the data for the end-user.

## Key Features

### Automated Infrastructure
* **Serverless Execution:** No backend server is required; the project runs entirely on GitHub's infrastructure.
* **Versioned History:** By storing data in the repository, I have a built-in version history of every data point collected.

### The Data Dashboard
I built a presentation layer to make the raw JSON data accessible and visually engaging. The dashboard features:
* **Modern UI:** A dark glassmorphism aesthetic featuring an animated count-up hero section.
* **Real-time Metrics:** A display of the current index value, a daily change pill, and calculated 30-day percentage changes.
* **Historical Context:** Automatic tracking of all-time highs and lows (including dates) and a total counter of days tracked.
* **Interactive Analytics:** A Chart.js visualization with a range selector allowing users to toggle between 1 Month, 3 Months, 6 Months, 1 Year, or the full history.
* **Detailed Logs:** A recent records table equipped with per-day trend tags.

## Potential Use Cases

This project serves as a blueprint for any scenario where lightweight, scheduled data collection is required. Some potential applications include:

* **Economic Monitoring:** For analysts who need a hands-off way to track the broad strength of the USD over long periods.
* **Algorithmic Inputs:** The `prices.json` file can serve as a lightweight API endpoint for other projects needing historical USD index data.
* **Educational Blueprint:** Demonstrating how to use GitHub Actions as a "cron job" and a Git repository as a flat-file database.

## Project Structure

I have organized the project to keep the logic separated from the data:

* `.github/workflows/update-dollar.yml`: The engine that drives the automation.
* `fetch-dollar.js`: The logic for API interaction and data cleaning.
* `index.html`: The client-side dashboard.
* `prices.json`: The historical ledger of index values.