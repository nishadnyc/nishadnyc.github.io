---
layout: post
title: "us-dollar-index"
date: 2026-11-08 15:51:26 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker Monitoring the strength of the U.S. Dollar is essen..."
---

# Building a Serverless U.S. Dollar Index Tracker

Monitoring the strength of the U.S. Dollar is essential for understanding global trade and economic trends. However, maintaining a historical record of this data usually requires a dedicated database, a backend server, and a manual process for data ingestion. 

I wanted to solve this by building a completely automated, serverless data pipeline. I created the **U.S. Dollar Index Tracker**, a system that leverages GitHub as both the execution engine and the storage layer to track the Nominal Broad U.S. Dollar Index without any manual intervention.

## What is the U.S. Dollar Index Tracker?

The U.S. Dollar Index Tracker is a lightweight application designed to continuously collect and store the **Nominal Broad U.S. Dollar Index** (Series: `DTWEXBGS`) from the Federal Reserve Economic Data (FRED) API. 

Unlike the common DXY futures contract, the Nominal Broad Index is a trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners, providing a more comprehensive view of the dollar's value.

## How the Architecture Works

My goal was to eliminate the need for a dedicated backend. I achieved this by utilizing a "Git-as-a-Database" approach. Here is the operational flow of the project:

1.  **Scheduled Automation**: I use GitHub Actions to trigger a workflow on a set schedule.
2.  **Data Retrieval**: A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3.  **Validation**: The system ignores invalid or unavailable observations to ensure data integrity.
4.  **Storage**: New observations are appended to a `prices.json` file. If FRED has no new observation for a specific day, the rate is carried forward to maintain a consistent daily record.
5.  **Version Control**: GitHub automatically commits the updated `prices.json` back to the repository.
6.  **Visualization**: A frontend dashboard (`index.html`) fetches this JSON file and renders the data using Chart.js.

## Key Features

### Automated Pipeline
The project is entirely self-running. By combining GitHub Actions with the FRED API, the system handles data fetching, duplicate-date protection, and storage automatically.

### High-Performance Dashboard
I developed a presentation layer that focuses on clarity and modern aesthetics. The dashboard includes:
*   **Glassmorphism UI**: A dark-themed interface with an animated count-up hero section.
*   **Real-time Insights**: A current index value accompanied by a daily change pill.
*   **Historical Metrics**: Tracking of 30-day change, percentage shifts, and all-time highs and lows (including the dates they occurred).
*   **Interactive Visualizations**: A Chart.js implementation featuring a range selector to filter data by 1 month, 3 months, 6 months, 1 year, or all-time.
*   **Data Transparency**: A recent records table with trend tags for a granular look at daily movements.

### Zero-Build Frontend
The dashboard is designed to be lightweight. There is no frontend build step required; it loads the stored data directly from the repository.

## Potential Use Cases

This project serves as a blueprint for anyone looking to track economic indicators or financial data without paying for cloud infrastructure. Some potential applications include:

*   **Economic Research**: Maintaining a clean, version-controlled history of Federal Reserve data for analysis.
*   **Portfolio Monitoring**: Tracking the broad dollar index to understand how currency fluctuations might impact international investments.
*   **Educational Templates**: Serving as a reference for implementing serverless data pipelines using GitHub Actions and JSON storage.

By treating the repository as both the server and the database, I have created a sustainable, low-overhead system for long-term economic data tracking.