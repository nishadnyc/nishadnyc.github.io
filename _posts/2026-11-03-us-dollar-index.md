---
layout: post
title: "us-dollar-index"
date: 2026-11-03 16:53:00 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker I wanted to create a system that could continuously..."
---

# Building a Serverless U.S. Dollar Index Tracker

I wanted to create a system that could continuously track the strength of the U.S. Dollar without the overhead of managing a dedicated server, a database, or manual data entry. To achieve this, I built the U.S. Dollar Index Tracker: an automated, serverless data pipeline that collects and stores the Nominal Broad U.S. Dollar Index.

## What is the U.S. Dollar Index Tracker?

The project is a self-sustaining data collection system designed to monitor the `DTWEXBGS` series from the Federal Reserve Economic Data (FRED) API. Specifically, it tracks the Nominal Broad U.S. Dollar Index—a trade-weighted measure of the U.S. dollar against the currencies of major trading partners.

My primary goal was to leverage GitHub as more than just a place to store code. In this architecture, GitHub serves two critical roles:
1. **The Execution Environment:** Using GitHub Actions to run scheduled tasks.
2. **The Storage Layer:** Using the repository itself as a versioned database for historical data.

## How the Pipeline Works

I designed the system to operate with zero human intervention. The process follows a precise automated loop:

1. **Trigger:** A GitHub Actions workflow runs on a predefined schedule.
2. **Fetch:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3. **Validate:** The system filters out invalid or unavailable observations to ensure data integrity.
4. **Store:** New observations are appended to a `prices.json` file. I've implemented duplicate-date protection and a logic that carries the last known rate forward when FRED provides no new observation for a calendar day.
5. **Commit:** GitHub automatically commits the updated JSON file back to the repository.
6. **Visualize:** The `index.html` dashboard pulls the latest data from the JSON file to render the UI.

## Key Features

### Automated Data Management
Because the project is serverless, there are no hosting fees or backend maintenance tasks. The historical record grows automatically over time, creating a permanent archive of the index.

### The Analytics Dashboard
I developed a lightweight, high-performance frontend to make the data accessible. The dashboard features:
*   **Glassmorphism Design:** A modern dark-themed interface with an animated count-up hero section.
*   **Real-time Metrics:** A current index value display featuring a daily change pill, 30-day change, and percentage shifts.
*   **Historical Context:** Automatic calculation of all-time highs and lows (including dates) and a counter for the total days tracked.
*   **Interactive Visualization:** A Chart.js integration that allows users to toggle between 1-month, 3-month, 6-month, 1-year, and "All" time ranges.
*   **Detailed Records:** A recent records table complete with per-day trend tags.

## Potential Use Cases

This architecture provides a blueprint for any project requiring lightweight time-series data collection. Beyond tracking the U.S. Dollar Index, this pattern can be used for:
*   **Economic Monitoring:** Tracking other FRED series like inflation rates, GDP, or unemployment.
*   **Price Tracking:** Monitoring specific commodity prices or exchange rates for personal financial analysis.
*   **Health Metrics:** Automating the collection of public health data for research dashboards.

By treating a Git repository as a database and GitHub Actions as a cron job, I've created a tool that is essentially "set and forget," providing a professional-grade data visualization tool with zero infrastructure cost.