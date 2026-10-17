---
layout: post
title: "us-dollar-index"
date: 2026-10-17 12:45:53 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker I wanted to create a system that could continuously..."
---

# Building a Serverless U.S. Dollar Index Tracker

I wanted to create a system that could continuously track the strength of the U.S. Dollar without the overhead of managing a dedicated server, a database, or manual data entry. To achieve this, I built the **U.S. Dollar Index Tracker**, an automated, serverless data pipeline that leverages GitHub as both its execution engine and its storage layer.

## What is the U.S. Dollar Index Tracker?

The U.S. Dollar Index Tracker is a specialized tool designed to collect and visualize the **Nominal Broad U.S. Dollar Index** (Series: `DTWEXBGS`). Unlike the more common DXY futures contract, this index is a broad, trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners, sourced directly from the Board of Governors of the Federal Reserve System via the FRED API.

The core of the project is a self-sustaining loop: a scheduled workflow fetches the data, a script processes it, and the repository itself stores the history.

## How the Pipeline Works

I designed the project to run with zero human intervention. Here is the technical flow of the data pipeline:

1.  **Scheduled Automation:** I use GitHub Actions to trigger a workflow on a set schedule.
2.  **Data Retrieval:** A Node.js script (`fetch-dollar.js`) requests the latest daily observation from the FRED API.
3.  **Data Validation:** To ensure data integrity, the system ignores invalid or unavailable observations.
4.  **Versioned Storage:** New observations are appended to `prices.json`. If FRED has no new observation for a specific day, the rate is carried forward to maintain a consistent daily record.
5.  **Automatic Commits:** Once the data is updated, GitHub Actions commits the changes back to the repository, effectively using Git as a versioned database.
6.  **Visualization:** A frontend dashboard (`index.html`) pulls this JSON file to render the data for the end user.

## Key Features

### Automated Infrastructure
*   **Serverless Execution:** By utilizing GitHub Actions, I eliminated the need for a backend server.
*   **Duplicate Protection:** I implemented automatic duplicate-date protection to ensure the historical record remains clean.
*   **No Build Step:** The frontend is designed to be lightweight, requiring no build process or compilation.

### The Analytics Dashboard
I developed a "dark glassmorphism" dashboard to make the data exploration intuitive and visually appealing. Key UI features include:
*   **Real-time Metrics:** An animated count-up hero section showing the current index value and a daily change pill.
*   **Performance Tracking:** Immediate visibility into 30-day changes and percentage shifts.
*   **Historical Context:** Tracking of all-time highs and lows (including dates) and a counter for the total days tracked.
*   **Interactive Visuals:** A Chart.js integration with a range selector allowing users to toggle between 1 Month, 3 Months, 6 Months, 1 Year, and All-time views.
*   **Detailed Logs:** A recent records table featuring per-day trend tags for quick scanning.

## Potential Use Cases

This architecture isn't just about currency; it's a blueprint for any project requiring lightweight, scheduled data collection. Potential use cases include:

*   **Financial Monitoring:** Tracking specific economic indicators or commodity prices that are updated daily.
*   **Personal Portfolio Tracking:** Automating the collection of asset prices into a version-controlled file.
*   **Public Data Archiving:** Creating a permanent, searchable record of API-based data that might otherwise be overwritten or difficult to access historically.

By combining GitHub Actions with a simple JSON storage strategy, I have created a tool that provides professional-grade data tracking with minimal maintenance.