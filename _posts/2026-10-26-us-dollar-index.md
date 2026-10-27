---
layout: post
title: "us-dollar-index"
date: 2026-10-26 15:57:17 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker I wanted to create a system that could continuously..."
---

# Building a Serverless U.S. Dollar Index Tracker

I wanted to create a system that could continuously monitor the strength of the U.S. Dollar without the overhead of managing a dedicated server, a database, or manual data entry. To achieve this, I developed the **U.S. Dollar Index Tracker**, an automated, serverless data pipeline that treats GitHub as both the execution engine and the storage layer.

## What is the U.S. Dollar Index Tracker?

This project is a self-sustaining pipeline designed to collect and store the **Nominal Broad U.S. Dollar Index** (Series: `DTWEXBGS`). Unlike the common DXY futures contract, this index is a trade-weighted measure provided by the Board of Governors of the Federal Reserve System, offering a broader view of the dollar's value against the currencies of major U.S. trading partners.

My goal was to build a "set-it-and-forget-it" system. By leveraging GitHub Actions and a simple JSON file for storage, I have eliminated the need for a traditional backend.

## How the Pipeline Works

The architecture is designed for maximum efficiency and zero maintenance. The workflow follows a continuous loop:

1. **Scheduled Execution:** A GitHub Action triggers a workflow on a set schedule.
2. **Data Retrieval:** The `fetch-dollar.js` script requests the most recent observation from the FRED API.
3. **Validation:** To ensure data integrity, any invalid or unavailable observations are automatically ignored.
4. **Persistent Storage:** New observations are appended to `prices.json`. I implemented duplicate-date protection to ensure the dataset remains clean.
5. **Automated Commits:** Once the data is updated, the GitHub Action commits the changes directly back to the repository.
6. **Visualization:** The `index.html` dashboard pulls the latest data from the JSON file and renders it for the user.

## Key Features

### Automated Data Management
* **Serverless Execution:** No dedicated backend is required; the entire project runs via GitHub Actions.
* **Versioned History:** Because the data is stored in the repository, I have a complete, version-controlled history of the index.
* **Data Continuity:** The system ensures one record per calendar day, carrying the previous rate forward when FRED has no new observation.

### The Analytics Dashboard
I built a lightweight, frontend-only dashboard that requires no build step. It features a dark glassmorphism aesthetic and provides several high-level insights:
* **Real-time Metrics:** An animated count-up hero section displaying the current index value and a daily change pill.
* **Trend Analysis:** Instant visibility into 30-day changes and percentage shifts.
* **Historical Context:** Tracking of all-time highs and lows, including the specific dates they occurred and a total "days-tracked" counter.
* **Interactive Visualization:** A Chart.js integration with a range selector allowing users to toggle between 1-month, 3-month, 6-month, 1-year, or "All" time views.
* **Detailed Logs:** A recent records table featuring per-day trend tags.

## Potential Use Cases

This project serves as a blueprint for anyone looking to track macroeconomic data without paying for cloud infrastructure. Potential use cases include:

* **Macroeconomic Research:** Maintaining a personal historical record of currency fluctuations for long-term analysis.
* **Financial Monitoring:** Creating a lightweight "heartbeat" monitor for the USD to inform trading or investment strategies.
* **Proof of Concept:** Demonstrating how GitHub can be utilized as a database and a cron-job scheduler for small-scale data engineering projects.