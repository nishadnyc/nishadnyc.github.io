---
layout: post
title: "us-dollar-index"
date: 2026-11-04 16:56:18 +0000
categories: projects
excerpt: "Tracking the U.S. Dollar Index: A Serverless Approach to Data Collection I have developed a fully a..."
---

# Tracking the U.S. Dollar Index: A Serverless Approach to Data Collection

I have developed a fully automated, serverless data pipeline designed to continuously collect and store the Nominal Broad U.S. Dollar Index. By leveraging modern CI/CD tools and a lightweight frontend, I have created a system that manages the entire lifecycle of data—from acquisition to visualization—without the need for a dedicated server or manual intervention.

## What is the U.S. Dollar Index Tracker?

The U.S. Dollar Index Tracker is a specialized tool that monitors the **Nominal Broad U.S. Dollar Index (Series: DTWEXBGS)** provided by the Federal Reserve Bank of St. Louis (FRED). Unlike the common DXY futures contract, this index serves as a broad, trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners.

The core purpose of this project was to experiment with using GitHub not just as a code repository, but as both an execution environment and a versioned storage layer.

## How the Pipeline Works

I designed the system to be entirely self-sustaining. The architecture follows a streamlined loop:

1.  **Scheduled Trigger:** GitHub Actions triggers a workflow on a set schedule.
2.  **Data Acquisition:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3.  **Validation:** To ensure data integrity, the system automatically ignores invalid or unavailable observations.
4.  **Storage:** New observations are appended to a `prices.json` file. I have implemented duplicate-date protection to ensure each calendar day has only one record; if FRED has no new observation for a day, the previous rate is carried forward.
5.  **Version Control:** GitHub commits the updated JSON file back to the repository, creating a permanent, versioned historical record.
6.  **Visualization:** The `index.html` dashboard loads this JSON data and renders it for the user.

## Key Features

### Automated Infrastructure
*   **Serverless Execution:** By using GitHub Actions, I eliminated the need for a backend server or cloud hosting fees.
*   **Zero-Maintenance:** Once configured, the pipeline runs independently without human intervention.
*   **Direct Repository Storage:** Historical data is stored as a flat file within the repo, making it portable and transparent.

### Advanced Dashboard
I built a presentation layer that focuses on clarity and modern aesthetics:
*   **Glassmorphism UI:** A dark-themed dashboard featuring an animated count-up hero section.
*   **Real-time Metrics:** The interface displays the current index value, a daily change pill, 30-day trends, and percentage changes.
*   **Historical Context:** I've included an all-time high/low tracker with associated dates and a counter for the total number of days tracked.
*   **Interactive Visuals:** Using Chart.js, the dashboard provides a detailed visualization with range selectors for 1 Month, 3 Months, 6 Months, 1 Year, and All-time views.
*   **Trend Analysis:** A recent records table allows users to see per-day trend tags at a glance.

## Potential Use Cases

This architecture provides a blueprint for various data-tracking needs:

*   **Economic Monitoring:** Tracking specific FRED series to analyze macroeconomic trends without paying for expensive data terminals.
*   **Portfolio Benchmarking:** Using the broad dollar index to understand how currency strength is impacting international investments.
*   **Educational Tooling:** Demonstrating how to build a "database-less" application where a JSON file in a Git repo serves as the primary data store.
*   **Lightweight API Mirrors:** Creating a simplified, cached version of an external API for public consumption through a static frontend.