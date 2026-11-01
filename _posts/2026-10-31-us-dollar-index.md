---
layout: post
title: "us-dollar-index"
date: 2026-10-31 15:10:23 +0000
categories: projects
excerpt: "Building a Serverless Pipeline for the U.S. Dollar Index Tracking macroeconomic indicators usually..."
---

# Building a Serverless Pipeline for the U.S. Dollar Index

Tracking macroeconomic indicators usually requires a choice between paying for an expensive data subscription or manually exporting CSVs from a government website. I wanted a middle ground: a system that automatically collects, stores, and visualizes the Nominal Broad U.S. Dollar Index without the overhead of managing a dedicated server or a database.

To achieve this, I built the **U.S. Dollar Index Tracker**, a completely serverless data pipeline that leverages GitHub as both the compute engine and the storage layer.

## What is the U.S. Dollar Index Tracker?

The U.S. Dollar Index Tracker is an automated system designed to monitor the Nominal Broad U.S. Dollar Index (`DTWEXBGS`). Unlike the commonly cited DXY futures contract, this project tracks the trade-weighted measure provided by the Board of Governors of the Federal Reserve System via the FRED API.

The core purpose of the project is to maintain a continuous, versioned historical record of the dollar's strength against major trading partners, presenting that data through a modern, interactive web dashboard.

## How the Pipeline Works

I designed this project to operate with zero human intervention. By utilizing GitHub Actions, I have turned a version control repository into a self-updating database.

1. **Scheduled Execution:** A GitHub Actions workflow triggers on a set schedule.
2. **Data Retrieval:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3. **Validation & Storage:** The system filters out invalid observations. New, valid data points are appended to `prices.json`. To ensure data integrity, I implemented duplicate-date protection and a logic that carries the previous rate forward if FRED has no new observation for a specific calendar day.
4. **Versioned Commits:** GitHub Actions automatically commits the updated `prices.json` back to the repository.
5. **Frontend Rendering:** An `index.html` file acts as the presentation layer, fetching the JSON data and rendering it via Chart.js.

## Key Features

### Automated Infrastructure
*   **Serverless Architecture:** No backend servers to maintain or monthly hosting bills.
*   **GitHub as Storage:** The repository serves as a versioned historical archive of the index.
*   **Automated Maintenance:** The pipeline handles its own updates and data cleaning.

### Advanced Visualization Dashboard
I wanted the data to be as visually engaging as it is accurate. The dashboard includes:
*   **Glassmorphism Design:** A dark-themed, modern interface with an animated count-up hero section.
*   **Real-time Metrics:** Current index value with a daily change "pill," 30-day change, and percentage shifts.
*   **Historical Context:** Automatic calculation of all-time highs and lows (with dates) and a total days-tracked counter.
*   **Interactive Charting:** Using Chart.js, I've included a range selector allowing users to toggle between 1 month, 3 months, 6 months, 1 year, or the full history.
*   **Detailed Records:** A recent records table featuring per-day trend tags for quick scanning.

## Potential Use Cases

This architecture provides a blueprint for anyone looking to track time-series data without complex infrastructure. Potential applications include:

*   **Macroeconomic Monitoring:** For traders or analysts who need a lightweight way to track currency trends.
*   **Educational Tooling:** As a demonstration of how to use GitHub Actions for "cron-job" style automation.
*   **Custom Indicator Tracking:** The same logic can be adapted to track any other FRED series, such as inflation rates (CPI) or unemployment data.

By treating the repository as a data store and GitHub Actions as the backend, I've created a sustainable, low-maintenance system that transforms raw API data into actionable visual insights.