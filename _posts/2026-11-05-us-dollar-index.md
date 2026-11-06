---
layout: post
title: "us-dollar-index"
date: 2026-11-05 17:11:47 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker Tracking macroeconomic indicators often requires ex..."
---

# Building a Serverless U.S. Dollar Index Tracker

Tracking macroeconomic indicators often requires expensive subscriptions or manual data exports. To solve this, I built the **U.S. Dollar Index Tracker**, an automated, serverless data pipeline designed to continuously collect and store the Nominal Broad U.S. Dollar Index without the need for a dedicated server or manual updates.

## What is the Project?

My project is a self-sustaining system that monitors the Nominal Broad U.S. Dollar Index (Series: `DTWEXBGS`) provided by the Federal Reserve Economic Data (FRED) API. Rather than relying on a traditional backend, I have leveraged GitHub as both the execution environment and the storage layer.

The system uses a combination of GitHub Actions for automation, a Node.js script for data retrieval, and a lightweight HTML/JavaScript dashboard for visualization.

## The Purpose: Serverless Data Collection

The primary goal of this project was to create a "set-and-forget" system. By using GitHub Actions, I can schedule data fetches that occur automatically in the cloud. When the script runs, it fetches the latest observation, validates it, and commits the updated data directly back into a `prices.json` file within the repository.

This approach effectively turns a Git repository into a versioned database, ensuring that I have a permanent historical record of the index while keeping overhead at zero.

## Key Features

### Automated Pipeline
*   **Scheduled Updates:** GitHub Actions triggers the `fetch-dollar.js` script on a set schedule.
*   **Intelligent Storage:** The system ensures one record per calendar day. If FRED has no new observation for a specific date, the rate is carried forward.
*   **Data Integrity:** I implemented automatic duplicate-date protection and filters to ignore invalid or unavailable observations.

### Modern Data Dashboard
I developed a frontend presentation layer that transforms raw JSON data into actionable insights. The dashboard features:
*   **Glassmorphism Design:** A sleek, dark-themed interface with an animated count-up hero section.
*   **Real-time Metrics:** A current index value display featuring a daily change pill, 30-day change metrics, and percentage shifts.
*   **Historical Context:** Tracking of all-time highs and lows (including dates) and a total "days-tracked" counter.
*   **Interactive Visualizations:** A Chart.js integration allowing users to toggle between 1-month, 3-month, 6-month, 1-year, and "All" time ranges.
*   **Detailed Logs:** A recent records table equipped with per-day trend tags.

## Data Source and Technical Specs

The project tracks the **Nominal Broad U.S. Dollar Index** from the Board of Governors of the Federal Reserve System. This is a trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners (indexed to January 2006 = 100). 

*Note: This project specifically tracks the FRED Nominal Broad Index, not the ICE U.S. Dollar Index futures contract (DXY).*

## Potential Use Cases

This architecture provides a blueprint for several different types of applications:

1.  **Macroeconomic Monitoring:** For traders or economists who need a historical record of trade-weighted currency strength without paying for a Bloomberg terminal.
2.  **Alternative Data Pipelines:** This "GitHub-as-a-Database" model can be applied to any public API (e.g., weather, stock prices, or census data) to create a free, versioned archive.
3.  **Portfolio Tracking:** Integrating this data into a broader financial dashboard to understand how a strong or weak dollar is impacting international asset valuations.