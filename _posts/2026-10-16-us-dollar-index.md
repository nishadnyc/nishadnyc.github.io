---
layout: post
title: "us-dollar-index"
date: 2026-10-16 13:48:43 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker Tracking macroeconomic indicators often requires ex..."
---

# Building a Serverless U.S. Dollar Index Tracker

Tracking macroeconomic indicators often requires expensive subscriptions or the manual effort of exporting CSVs. I wanted a way to monitor the Nominal Broad U.S. Dollar Index continuously without managing a dedicated server or paying for a backend. 

To solve this, I built the **U.S. Dollar Index Tracker**: an automated, serverless data pipeline that leverages GitHub as both a compute engine and a database.

## What is the U.S. Dollar Index Tracker?

My project is a fully automated system designed to collect and store the Nominal Broad U.S. Dollar Index (Series: `DTWEXBGS`) from the Federal Reserve Economic Data (FRED) API. Unlike the common DXY futures contract, this tracker focuses on the trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners, as provided by the Board of Governors of the Federal Reserve System.

The core philosophy of this project is "set it and forget it." By utilizing GitHub Actions, I have eliminated the need for a traditional server. The system wakes up on a schedule, fetches the latest data, and commits it directly back into the repository.

## How the Pipeline Works

I designed the architecture to be lightweight and circular. Here is the step-by-step flow of how the data moves from the Federal Reserve to the browser:

1. **Trigger:** A GitHub Actions workflow runs on a predefined schedule.
2. **Extraction:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3. **Validation:** The system ignores invalid or unavailable observations to ensure data integrity.
4. **Storage:** New observations are appended to a `prices.json` file. To maintain a consistent timeline, I implemented a logic where the rate is carried forward if FRED has no new observation for a specific day.
5. **Persistence:** GitHub automatically commits the updated `prices.json` back to the repository, creating a versioned historical record.
6. **Visualization:** The `index.html` dashboard fetches this JSON file and renders it using Chart.js.

## Key Features

### Serverless Automation
Because I am using GitHub Actions for execution and the repository for storage, there are zero hosting costs and no backend maintenance.

### Data Integrity
I've implemented automatic duplicate-date protection to ensure the historical record remains clean and accurate, with exactly one record per calendar day.

### High-Performance Dashboard
The frontend is a lightweight, no-build-step interface featuring:
* **Dark Glassmorphism Design:** A modern aesthetic with an animated count-up hero section.
* **Real-time Metrics:** Current index value, daily change pills, and 30-day percentage changes.
* **Historical Context:** All-time highs and lows with associated dates and a total "days-tracked" counter.
* **Interactive Visuals:** A Chart.js visualization with customizable time ranges (1 Month, 3 Months, 6 Months, 1 Year, and All).
* **Detailed Logs:** A recent records table with per-day trend tags for quick scanning.

## Potential Use Cases

This project serves as a blueprint for anyone looking to track financial or economic time-series data without infrastructure overhead. Some potential use cases include:

* **Macroeconomic Analysis:** Monitoring the strength of the USD to understand its impact on global trade and commodity pricing.
* **Portfolio Correlation:** Comparing the Broad Dollar Index against other assets to identify inverse correlations.
* **Educational Framework:** Using this as a reference architecture for building other "GitOps" data pipelines (e.g., tracking stock prices, weather data, or API health).

## Project Structure

I have organized the project to keep the automation logic separate from the presentation layer:

* `.github/workflows/update-dollar.yml`: The heartbeat of the project; handles the scheduled automation.
* `fetch-dollar.js`: The logic for API communication and data sanitization.
* `prices.json`: The "database" containing all historical observations.
* `index.html`: The presentation layer and user interface.