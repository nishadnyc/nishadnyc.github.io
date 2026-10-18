---
layout: post
title: "us-dollar-index"
date: 2026-10-18 13:04:17 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker Monitoring the strength of the U.S. Dollar is essen..."
---

# Building a Serverless U.S. Dollar Index Tracker

Monitoring the strength of the U.S. Dollar is essential for understanding global trade dynamics and currency fluctuations. To track this effectively without the overhead of maintaining a dedicated server or manually downloading CSVs, I built the **U.S. Dollar Index Tracker**.

This project is an automated, serverless data pipeline designed to continuously collect and store the Nominal Broad U.S. Dollar Index. By leveraging modern CI/CD tools as a backend, I have created a system that manages its own data collection and visualization.

## The Purpose: Automation Through Serverless Architecture

The core objective of this project was to experiment with using GitHub as more than just a code repository. I wanted to utilize GitHub as both the execution environment and the versioned storage layer. 

By integrating GitHub Actions with the FRED (Federal Reserve Economic Data) API, I eliminated the need for a traditional database or a hosted backend. The system is entirely self-running; once configured, it requires zero human intervention to maintain a comprehensive historical record of currency data.

## How the Pipeline Works

I designed the workflow to be lightweight and resilient. The process follows a six-step automated cycle:

1. **Scheduled Trigger:** A GitHub Action runs on a predefined schedule.
2. **Data Retrieval:** The `fetch-dollar.js` script requests the latest observation from the FRED API.
3. **Validation:** The system filters out invalid or unavailable observations to ensure data integrity.
4. **Storage:** New observations are appended to `prices.json`.
5. **Version Control:** GitHub automatically commits the updated JSON file back to the repository.
6. **Visualization:** The `index.html` dashboard pulls this JSON data and renders it via Chart.js.

## Key Features

### Automated Data Collection
I implemented a robust pipeline that ensures one record per calendar day. If FRED has no new observation for a specific date, the system carries the previous rate forward, ensuring a continuous timeline without gaps. It also includes built-in protection to prevent duplicate entries for the same date.

### The Data Source
The tracker focuses on the **Nominal Broad U.S. Dollar Index (Series: DTWEXBGS)** from the Board of Governors of the Federal Reserve System. This is a trade-weighted measure of the USD against major trading partners, providing a broader economic picture than the standard DXY futures contract.

### High-Performance Dashboard
To make the data accessible, I developed a frontend dashboard featuring:
* **Modern UI:** A dark glassmorphism design with an animated count-up hero section.
* **Instant Metrics:** A current index value display accompanied by a daily change pill, 30-day percentage changes, and all-time high/low markers.
* **Interactive Visualization:** A Chart.js implementation with a range selector allowing users to toggle between 1 Month, 3 Months, 6 Months, 1 Year, and All-time views.
* **Detailed Logs:** A recent records table with per-day trend tags for granular analysis.
* **Zero Build Step:** The frontend is designed to run directly in the browser without requiring a build process.

## Potential Use Cases

This architecture provides a blueprint for various data-tracking needs:

* **Financial Monitoring:** Tracking specific economic indicators or indices that are updated daily.
* **Portfolio Benchmarking:** Using the broad dollar index to correlate currency strength with asset performance.
* **Educational Tooling:** Demonstrating how to build "low-ops" applications that combine API orchestration with git-based storage.
* **Lightweight Reporting:** Creating public-facing dashboards for niche data sets without incurring cloud hosting costs.