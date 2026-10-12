---
layout: post
title: "us-dollar-index"
date: 2026-10-12 08:31:47 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker Monitoring the strength of the U.S. Dollar is essen..."
---

# Building a Serverless U.S. Dollar Index Tracker

Monitoring the strength of the U.S. Dollar is essential for understanding global trade dynamics and currency trends. I wanted to create a system that could track this data continuously without the overhead of managing a dedicated server, paying for database hosting, or performing manual data entries. 

To achieve this, I built the **U.S. Dollar Index Tracker**, an automated, serverless data pipeline that collects and stores the Nominal Broad U.S. Dollar Index.

## The Core Concept

The primary goal of this project was to leverage GitHub not just as a place to store code, but as a complete infrastructure. I have designed the system to use GitHub as both the execution environment and the versioned storage layer.

By utilizing GitHub Actions, I can run a scheduled workflow that fetches data, processes it, and commits it back to the repository. This transforms a simple Git repo into a self-sustaining database and application host.

## How the Pipeline Works

I have structured the data flow to be entirely hands-off. Once configured, the process follows these steps:

1. **Scheduled Trigger:** A GitHub Actions workflow runs on a set schedule.
2. **Data Fetching:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the Federal Reserve Economic Data (FRED) API.
3. **Validation:** The system ignores invalid or unavailable observations to ensure data integrity.
4. **Storage:** New observations are appended to `prices.json`. To maintain a consistent timeline, I implemented a logic where the rate is carried forward if FRED has no new observation for a specific day.
5. **Persistence:** GitHub automatically commits the updated `prices.json` back to the repository.
6. **Visualization:** The `index.html` dashboard reads this JSON file and renders the data for the user.

## Key Features

### Automated Data Infrastructure
* **Serverless Execution:** No backend server is required; the entire pipeline runs on GitHub's infrastructure.
* **Duplicate Protection:** I've integrated automatic duplicate-date protection to prevent redundant entries.
* **Historical Archiving:** Every data point is versioned within the repository, providing a transparent history of the index.

### Interactive Dashboard
I wanted the presentation layer to be as polished as the pipeline. The dashboard features a dark glassmorphism design and includes:
* **Real-time Metrics:** An animated count-up hero section displaying the current index value and a daily change pill.
* **Performance Analytics:** Calculation of 30-day change, percentage shifts, and all-time highs/lows including the specific dates they occurred.
* **Advanced Visualization:** An interactive Chart.js graph with a range selector allowing users to toggle between 1 month, 3 months, 6 months, 1 year, or the entire history.
* **Detailed Logs:** A recent records table with per-day trend tags for quick scanning.

## Data Source and Use Cases

I am specifically tracking the **Nominal Broad U.S. Dollar Index (Series: DTWEXBGS)** sourced from the Board of Governors of the Federal Reserve System. Unlike the commonly cited DXY (ICE U.S. Dollar Index), this is a broad, trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners.

### Potential Use Cases
This project serves as a blueprint for several types of applications:
* **Economic Monitoring:** For analysts who need a lightweight, always-on tracker for Federal Reserve data.
* **Algorithmic Inputs:** The `prices.json` file can be consumed by other scripts or bots as a reliable, historical data source.
* **Educational Tool:** A demonstration of how to build "low-code" serverless pipelines using Git-based storage.

## Project Structure

The project is organized to keep the logic separate from the presentation:

* `.github/workflows/update-dollar.yml`: The heartbeat of the project (automation).
* `fetch-dollar.js`: The logic for API communication and data cleaning.
* `index.html`: The frontend dashboard.
* `prices.json`: The historical dataset.