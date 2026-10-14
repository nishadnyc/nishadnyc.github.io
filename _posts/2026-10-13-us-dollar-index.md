---
layout: post
title: "us-dollar-index"
date: 2026-10-13 16:47:13 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker I have developed a fully automated, serverless data..."
---

# Building a Serverless U.S. Dollar Index Tracker

I have developed a fully automated, serverless data pipeline designed to continuously collect and store the Nominal Broad U.S. Dollar Index. My goal was to create a system that operates with zero human intervention, leveraging GitHub as both the execution engine and the storage layer.

## What is the U.S. Dollar Index Tracker?

The U.S. Dollar Index Tracker is a lightweight system that monitors the `DTWEXBGS` series from the Federal Reserve Economic Data (FRED) API. This specific index is a nominal broad, trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners.

It is important to note that this project tracks the broad index provided by the Board of Governors of the Federal Reserve System, rather than the ICE U.S. Dollar Index futures contract (DXY).

## Purpose and Architecture

The primary objective of this project was to eliminate the need for a dedicated backend server. By utilizing a serverless architecture, I have shifted the operational burden to GitHub.

### How the Pipeline Works
I have structured the workflow into a seamless loop:
1. **Automation:** A GitHub Action triggers a scheduled workflow.
2. **Collection:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3. **Validation:** The system ignores invalid or unavailable observations to ensure data integrity.
4. **Persistence:** New observations are appended to a `prices.json` file.
5. **Version Control:** GitHub automatically commits the updated JSON file back to the repository, creating a permanent, versioned historical record.
6. **Visualization:** A frontend dashboard (`index.html`) fetches this JSON data to present it to the user.

## Key Features

### Automated Data Management
*   **Zero Maintenance:** Once configured, the pipeline runs on its own.
*   **Daily Records:** I ensure one record per calendar day; if FRED has no new observation, the rate is carried forward.
*   **Collision Prevention:** The system includes automatic duplicate-date protection to prevent redundant entries.

### Interactive Dashboard
To make the data accessible, I built a modern web dashboard featuring:
*   **Dark Glassmorphism Design:** A sleek UI with an animated count-up hero section.
*   **Real-time Metrics:** Displays the current index value, a daily change pill, and the 30-day percentage change.
*   **Historical Context:** Tracking of all-time highs and lows (including dates) and a total days-tracked counter.
*   **Advanced Visualization:** An interactive Chart.js graph with a range selector allowing users to toggle between 1 Month, 3 Months, 6 Months, 1 Year, and All-time views.
*   **Detailed Logs:** A recent records table equipped with per-day trend tags.
*   **No-Build Frontend:** The dashboard is designed to work without a complex frontend build step.

## Potential Use Cases

This project serves as a blueprint for anyone looking to build "set-and-forget" data trackers. Some potential applications include:
*   **Economic Monitoring:** Tracking currency strength to inform macroeconomic analysis.
*   **Portfolio Correlation:** Analyzing how the broad U.S. Dollar Index correlates with other asset classes.
*   **Educational Template:** Serving as a reference for implementing serverless pipelines using GitHub Actions and JSON-based storage.