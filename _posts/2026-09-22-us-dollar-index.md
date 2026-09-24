---
layout: post
title: "us-dollar-index"
date: 2026-09-22 13:52:31 +0000
categories: projects
excerpt: "Building a Serverless Pipeline for the U.S. Dollar Index Tracker Monitoring the strength of the U.S..."
---

# Building a Serverless Pipeline for the U.S. Dollar Index Tracker

Monitoring the strength of the U.S. Dollar is essential for understanding global trade dynamics and currency trends. I wanted a way to track the Nominal Broad U.S. Dollar Index without the overhead of managing a dedicated server, paying for hosting, or manually updating spreadsheets. 

To solve this, I built the **U.S. Dollar Index Tracker**: an automated, serverless data pipeline that handles everything from data ingestion to visualization.

## What is the U.S. Dollar Index Tracker?

The U.S. Dollar Index Tracker is a lightweight system designed to continuously collect and store the Nominal Broad U.S. Dollar Index (`DTWEXBGS`). Unlike the commonly cited DXY futures contract, this project tracks the broad, trade-weighted measure provided by the Board of Governors of the Federal Reserve System via the FRED API.

The core philosophy of this project was to leverage GitHub as more than just a place to store code. I have utilized GitHub as both the execution environment and the versioned storage layer for my data.

## How the Pipeline Works

I designed the project to be entirely "hands-off." Once configured, the system operates without any human intervention through the following workflow:

1. **Scheduled Execution:** A GitHub Action triggers a workflow on a predefined schedule.
2. **Data Retrieval:** A Node.js script (`fetch-dollar.js`) requests the latest daily observation from the FRED API.
3. **Validation:** The system automatically filters out invalid or unavailable observations to ensure data integrity.
4. **Storage:** New observations are appended to a `prices.json` file. To prevent data corruption, I implemented duplicate-date protection.
5. **Version Control:** GitHub commits the updated JSON file back to the repository, creating a permanent, versioned historical record.
6. **Visualization:** A frontend dashboard (`index.html`) fetches this JSON file and renders the data using Chart.js.

## Key Features

I focused on making the project efficient and the data accessible. Key features include:

*   **Zero-Infrastructure Backend:** By using GitHub Actions, I eliminated the need for a dedicated backend server.
*   **Automatic Data Persistence:** Historical data is stored directly in the repository, making it easy to audit and backup.
*   **Interactive Dashboard:** The integrated web interface provides a visual representation of the index over time.
*   **Advanced Analytics:** The dashboard doesn't just show a graph; it calculates:
    *   The current index value and daily change.
    *   30-day change and percentage fluctuations.
    *   All-time historical high and low values.
*   **Build-Free Frontend:** The dashboard is lightweight and requires no build step, allowing it to be served as a static page.

## Potential Use Cases

This architecture serves as a blueprint for any project requiring scheduled data collection from a public API. Potential use cases include:

*   **Economic Monitoring:** Tracking specific Federal Reserve series to analyze macroeconomic trends.
*   **Portfolio Tracking:** Monitoring currency fluctuations that impact international investments.
*   **Educational Tooling:** Demonstrating how to build a "serverless" data pipeline using Git-based storage.
*   **Custom Indexing:** Adapting the pipeline to track other FRED series or similar financial APIs.

By combining GitHub Actions for logic and a JSON file for storage, I've created a resilient, free, and automated system for tracking the strength of the U.S. Dollar.