---
layout: post
title: "us-dollar-index"
date: 2026-10-25 13:36:52 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker Monitoring the strength of the U.S. Dollar is essen..."
---

# Building a Serverless U.S. Dollar Index Tracker

Monitoring the strength of the U.S. Dollar is essential for understanding global trade dynamics and macroeconomic trends. To simplify this process, I have built an automated, serverless data pipeline that continuously collects and stores the Nominal Broad U.S. Dollar Index.

My primary goal was to create a self-running system that leverages GitHub as both the execution environment and the versioned storage layer, eliminating the need for a dedicated server or manual data entry.

## What is the Nominal Broad U.S. Dollar Index?

This project tracks the **Nominal Broad U.S. Dollar Index** (FRED series `DTWEXBGS`), sourced from the Board of Governors of the Federal Reserve System. 

Unlike the commonly cited DXY (the ICE U.S. Dollar Index futures contract), the Nominal Broad Index is a trade-weighted measure. It evaluates the U.S. dollar against the currencies of major U.S. trading partners, providing a more comprehensive view of the dollar's value in the context of international trade. The index is measured daily, with January 2006 serving as the baseline (100).

## How the Pipeline Works

I designed the project to operate with zero human intervention. The system follows a streamlined, serverless workflow:

1. **Scheduled Execution:** GitHub Actions triggers a scheduled workflow to run the update process.
2. **Data Retrieval:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3. **Validation:** The system ignores invalid or unavailable observations to ensure data integrity.
4. **Storage:** New observations are appended to `prices.json`. To maintain a continuous timeline, the rate is carried forward when FRED has no new observation for a specific day.
5. **Version Control:** GitHub automatically commits the updated JSON file back to the repository, creating a permanent historical record.
6. **Visualization:** The `index.html` dashboard loads this JSON file and renders the data for the end user.

## Key Features

### Automated Infrastructure
By utilizing GitHub Actions and JSON-based storage, I have removed the need for a traditional backend or database. The project is entirely serverless, meaning there are no hosting costs or server maintenance requirements.

### Intelligent Data Management
To ensure the dataset remains clean and useful, I implemented:
* **Duplicate Protection:** Automatic checks to prevent duplicate entries for the same date.
* **Continuous Tracking:** A logic that ensures one record per calendar day.
* **Historical Versioning:** Because data is stored in Git, every single update is version-tracked.

### High-Performance Dashboard
The presentation layer is a lightweight web dashboard that requires no frontend build step. It features:
* **Modern UI:** A dark glassmorphism design with an animated count-up hero section.
* **Real-time Metrics:** A current index value display with a daily change "pill," 30-day change metrics, and all-time high/low markers.
* **Interactive Analytics:** A Chart.js visualization with customizable range selectors (1 Month, 3 Months, 6 Months, 1 Year, and All-time).
* **Detailed Logs:** A recent records table featuring per-day trend tags for quick analysis.

## Potential Use Cases

This system serves as a foundation for several financial and technical applications:

* **Macroeconomic Monitoring:** For traders or economists who need a trade-weighted view of the USD without manually querying API endpoints.
* **Educational Templates:** As a blueprint for other "serverless data scrapers" that use GitHub as a database.
* **Custom Alerting:** The pipeline can be extended to trigger notifications when the index hits specific all-time highs or lows.
* **Historical Analysis:** Providing a clean, JSON-formatted dataset for those performing long-term correlation studies between the USD and other asset classes.