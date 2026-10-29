---
layout: post
title: "us-dollar-index"
date: 2026-10-28 15:48:18 +0000
categories: projects
excerpt: "Building a Serverless Pipeline for the U.S. Dollar Index I have developed a fully automated, server..."
---

# Building a Serverless Pipeline for the U.S. Dollar Index

I have developed a fully automated, serverless data pipeline designed to continuously collect and store the Nominal Broad U.S. Dollar Index. My goal was to create a system that functions entirely on its own—no dedicated servers, no manual data entry, and no expensive backend infrastructure.

By leveraging GitHub as both an execution environment and a storage layer, I’ve built a project that tracks currency strength over time while providing a modern interface for data exploration.

## What is the U.S. Dollar Index Tracker?

The U.S. Dollar Index Tracker is a specialized tool that monitors the **Nominal Broad U.S. Dollar Index** (Series `DTWEXBGS`). This specific index, provided by the Board of Governors of the Federal Reserve System, offers a broad, trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners.

It is important to note that this project specifically tracks the Federal Reserve's broad index rather than the ICE U.S. Dollar Index futures contract (DXY).

## How the Architecture Works

I designed this project to be completely hands-off. The pipeline operates through a series of automated steps:

1.  **Scheduled Execution:** I use GitHub Actions to trigger a workflow on a set schedule.
2.  **Data Fetching:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3.  **Validation:** The system filters out invalid or unavailable observations to ensure data integrity.
4.  **Storage:** New observations are appended to a `prices.json` file. To maintain a consistent timeline, I implemented a feature where the rate is carried forward if FRED has no new observation for a specific day.
5.  **Version Control:** GitHub commits the updated JSON file back to the repository, effectively using Git as a versioned database for historical records.
6.  **Visualization:** A lightweight frontend (`index.html`) reads the JSON file and renders the data via Chart.js.

## Key Features

### Automated Pipeline
The core of the project is its serverless nature. Because it relies on GitHub Actions and a JSON file for storage, there is no database to maintain and no server to patch.

### Data Integrity
To ensure the dataset remains clean, I built in automatic duplicate-date protection. This ensures that each calendar day has exactly one record, providing a seamless timeline for analysis.

### The Analytics Dashboard
I wanted the presentation layer to be as polished as the pipeline. The dashboard features:
*   **Modern UI:** A dark glassmorphism aesthetic with an animated count-up hero section.
*   **Instant Metrics:** A current index value with a daily change "pill," 30-day change tracking (absolute and percentage), and all-time high/low markers with corresponding dates.
*   **Interactive Visualization:** A Chart.js integration allowing users to toggle between 1 month, 3 months, 6 months, 1 year, or the entire history.
*   **Detailed Logs:** A recent records table featuring per-day trend tags.

## Potential Use Cases

This project serves as a blueprint for several different types of applications:

*   **Economic Monitoring:** For those who need to track the long-term strength of the USD without manually downloading CSVs from FRED.
*   **Portfolio Analysis:** Using the historical data to correlate currency strength with other asset classes.
*   **Technical Demonstration:** As a proof-of-concept for "GitOps" style data collection, demonstrating how a repository can act as both a database and a hosting platform.

## Project Structure

For those interested in the technical layout, here is how I organized the repository:

*   `.github/workflows/update-dollar.yml`: The heart of the automation.
*   `fetch-dollar.js`: The logic for API communication and data storage.
*   `index.html`: The dashboard interface.
*   `prices.json`: The historical data store.
*   `package.json`: Project metadata and dependencies.