---
layout: post
title: "us-dollar-index"
date: 2026-11-01 15:52:24 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker Tracking economic indicators often requires a choic..."
---

# Building a Serverless U.S. Dollar Index Tracker

Tracking economic indicators often requires a choice between expensive paid terminals or manually downloading CSV files. To solve this, I built the **U.S. Dollar Index Tracker**, an automated, serverless data pipeline that continuously collects and stores the Nominal Broad U.S. Dollar Index without the need for a dedicated server or manual updates.

## What is the U.S. Dollar Index Tracker?

The U.S. Dollar Index Tracker is a self-sustaining system designed to monitor the Nominal Broad U.S. Dollar Index (`DTWEXBGS`) provided by the Federal Reserve Economic Data (FRED) API. 

Unlike the commonly cited DXY futures contract, this project tracks the trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners. By leveraging GitHub as both an execution environment and a storage layer, I have created a pipeline that handles everything from data ingestion to visualization.

## How the Pipeline Works

I designed this project to run with zero human intervention. The architecture relies on a serverless loop:

1. **Scheduled Execution:** GitHub Actions triggers a workflow on a set schedule.
2. **Data Retrieval:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3. **Validation:** The system filters out invalid or unavailable observations to ensure data integrity.
4. **Persistence:** New observations are appended to `prices.json`. If FRED has no new observation for the day, the rate is carried forward to maintain a consistent daily record.
5. **Version Control:** GitHub automatically commits the updated JSON file back to the repository, creating a versioned historical archive.
6. **Visualization:** A frontend dashboard (`index.html`) loads this JSON data directly to present it to the user.

## Key Features

### Automated Infrastructure
*   **Serverless Execution:** No backend hosting costs; the entire logic resides in GitHub Actions.
*   **Direct Repository Storage:** Historical data is stored as a JSON file within the repo, serving as a lightweight database.
*   **Duplicate Protection:** Built-in safeguards prevent the same date from being recorded multiple times.

### Interactive Dashboard
I developed a modern, dark-themed glassmorphism dashboard to make the data accessible. Key UI features include:
*   **Animated Hero Section:** An animated count-up display showing the current index value.
*   **Instant Insights:** Daily change "pills," 30-day percentage changes, and all-time high/low records with associated dates.
*   **Dynamic Visualization:** An interactive Chart.js graph with a range selector allowing users to toggle between 1 Month, 3 Months, 6 Months, 1 Year, or All-time views.
*   **Detailed Logs:** A recent records table featuring per-day trend tags.
*   **Zero Build Step:** The frontend is pure HTML/JS, meaning no complex build pipeline is required to deploy the dashboard.

## Potential Use Cases

This project serves as a blueprint for anyone looking to track time-series economic data. Potential applications include:

*   **Economic Analysis:** Monitoring the strength of the USD to understand its impact on international trade and commodity pricing.
*   **Portfolio Monitoring:** Keeping a pulse on currency fluctuations that may affect foreign investments.
*   **Educational Framework:** Using the project as a template to track other FRED series by simply changing the series ID.

## Project Structure

The project is organized for maximum simplicity:

*   `.github/workflows/update-dollar.yml`: The automation heart of the project.
*   `fetch-dollar.js`: The logic for API communication and data storage.
*   `index.html`: The presentation layer.
*   `prices.json`: The historical data store.