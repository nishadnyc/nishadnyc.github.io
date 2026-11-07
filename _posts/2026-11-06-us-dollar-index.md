---
layout: post
title: "us-dollar-index"
date: 2026-11-06 16:38:33 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker I wanted to create a system that could continuously..."
---

# Building a Serverless U.S. Dollar Index Tracker

I wanted to create a system that could continuously track the Nominal Broad U.S. Dollar Index without the overhead of managing a dedicated server or performing manual data entries. The result is a fully automated, serverless data pipeline that leverages GitHub as both an execution environment and a versioned storage layer.

## What is the U.S. Dollar Index Tracker?

This project is an automated pipeline designed to collect and store data for the **Nominal Broad U.S. Dollar Index** (FRED series `DTWEXBGS`). This specific index provides a broad, trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners, sourced from the Board of Governors of the Federal Reserve System.

It is important to note that this project focuses on the broad index from FRED and does not track the ICE U.S. Dollar Index futures contract (DXY).

## How the Pipeline Works

I designed this project to run with zero human intervention. By utilizing a serverless architecture, the system follows a continuous loop of data acquisition and storage:

1. **Scheduled Execution:** GitHub Actions triggers a scheduled workflow.
2. **Data Retrieval:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3. **Validation:** The system filters out invalid or unavailable observations to ensure data integrity.
4. **Storage:** New observations are appended to a `prices.json` file. To maintain a consistent timeline, if FRED has no new observation for a specific day, the previous rate is carried forward.
5. **Version Control:** GitHub automatically commits the updated `prices.json` back to the repository, creating a permanent, versioned historical record.
6. **Visualization:** A frontend interface (`index.html`) pulls this JSON data to display it to the user.

## Key Features

### Automated Infrastructure
- **Serverless Backend:** No dedicated hosting is required; the entire backend logic resides in GitHub Actions.
- **Duplicate Protection:** The system includes built-in logic to prevent duplicate entries for the same calendar date.
- **Automated Versioning:** Since data is stored in the repository, I have a full audit trail of every data point collected.

### Interactive Dashboard
I built a lightweight web dashboard to make the data accessible and visually engaging. Key UI features include:
- **Modern Aesthetics:** A dark glassmorphism design featuring an animated count-up hero section.
- **Real-time Insights:** A current index value display accompanied by a daily change pill.
- **Analytical Metrics:** Automatic calculation of 30-day change, percentage shifts, and all-time highs and lows (including dates).
- **Advanced Visualization:** An interactive Chart.js graph with a range selector allowing users to toggle between 1 month, 3 months, 6 months, 1 year, or the entire history.
- **Detailed Records:** A recent records table that includes per-day trend tags.

## Potential Use Cases

This architecture serves as a blueprint for several types of data-tracking projects:

*   **Economic Monitoring:** Tracking specific federal indices or inflation markers over long periods.
*   **Asset Tracking:** Monitoring the price of a specific commodity or currency pair where a daily snapshot is sufficient.
*   **Lightweight Archiving:** Creating a historical archive of any API-provided data that does not offer a comprehensive historical export.
*   **Portfolio Dashboards:** Serving as a backend for personal financial dashboards that require daily updates without the cost of a cloud database.