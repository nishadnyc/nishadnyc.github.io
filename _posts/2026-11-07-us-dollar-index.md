---
layout: post
title: "us-dollar-index"
date: 2026-11-07 15:30:41 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker I have developed a fully automated, serverless data..."
---

# Building a Serverless U.S. Dollar Index Tracker

I have developed a fully automated, serverless data pipeline designed to continuously collect and store the Nominal Broad U.S. Dollar Index. My goal was to create a system that functions entirely on its own—no dedicated servers, no manual data entry, and no expensive backend infrastructure.

By leveraging the power of GitHub Actions and the FRED API, I've built a tool that treats a GitHub repository as both the execution environment and the versioned storage layer for financial data.

## What is the Nominal Broad U.S. Dollar Index?

For this project, I am tracking the `DTWEXBGS` series from the Federal Reserve Economic Data (FRED). This is a broad, trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners. Unlike the commonly cited DXY futures contract, this index provides a comprehensive view of the dollar's strength relative to a wider basket of currencies.

## How the Pipeline Works

I designed the architecture to be lightweight and maintenance-free. The workflow operates in a continuous loop:

1. **Scheduled Execution:** A GitHub Action triggers a scheduled workflow.
2. **Data Fetching:** I use a Node.js script (`fetch-dollar.js`) to request the latest daily observation from the FRED API.
3. **Validation:** The system automatically ignores invalid or unavailable observations to ensure data integrity.
4. **Storage:** New observations are appended to a `prices.json` file. I've implemented duplicate-date protection to ensure each calendar day has exactly one record; if FRED has no new observation for a specific day, the rate is carried forward.
5. **Persistence:** GitHub automatically commits the updated JSON file back to the repository, creating a permanent historical record.
6. **Visualization:** An `index.html` file reads this JSON file and renders the data instantly via Chart.js.

## Key Features

### Automated Data Engine
The core of the project is its autonomy. Because it is serverless, there are no monthly hosting fees or servers to manage. The data collection happens in the background, and the version history of the repository serves as a backup of the index's movement over time.

### Interactive Dashboard
To make the data accessible, I built a modern web dashboard featuring:
* **Glassmorphism Design:** A sleek, dark-themed interface with an animated count-up hero section.
* **Real-time Metrics:** An immediate view of the current index value, including a daily change pill, 30-day percentage changes, and all-time highs and lows with their respective dates.
* **Advanced Visualization:** An interactive chart powered by Chart.js, allowing users to filter data by 1 month, 3 months, 6 months, 1 year, or the entire history.
* **Trend Analysis:** A recent records table equipped with per-day trend tags for quick scanning.
* **Zero Build Step:** The frontend is pure HTML/JS, meaning it requires no compilation or build process to deploy.

## Potential Use Cases

This architecture provides a blueprint for any project requiring persistent time-series data without a database. Potential applications include:

* **Economic Monitoring:** Tracking specific inflation markers or employment figures from government APIs.
* **Portfolio Tracking:** Monitoring the price of a specific asset or commodity over months and years.
* **KPI Dashboards:** Automating the collection of public-facing business metrics for a simple internal status page.

By combining GitHub Actions for compute and a JSON file for storage, I have eliminated the complexity of traditional database management while maintaining a reliable, historical record of the U.S. Dollar Index.