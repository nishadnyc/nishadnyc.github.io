---
layout: post
title: "us-dollar-index"
date: 2026-10-27 15:12:40 +0000
categories: projects
excerpt: "Building a Serverless Pipeline for the U.S. Dollar Index Tracking financial indices usually require..."
---

# Building a Serverless Pipeline for the U.S. Dollar Index

Tracking financial indices usually requires a dedicated server, a database, and a constant maintenance schedule. I wanted to see if I could bypass that complexity entirely. I have built a U.S. Dollar Index Tracker—an automated, serverless data pipeline that continuously collects and stores the Nominal Broad U.S. Dollar Index without requiring a single piece of dedicated infrastructure.

## What is this Project?

The U.S. Dollar Index Tracker is a self-sustaining system designed to monitor the Nominal Broad U.S. Dollar Index (`DTWEXBGS`), provided by the Federal Reserve Economic Data (FRED) API. 

Rather than relying on a traditional backend, I leveraged GitHub as both the execution environment and the storage layer. By combining GitHub Actions with a versioned JSON file, I created a loop where the system fetches, validates, and stores financial data automatically.

## How the Pipeline Works

I designed the workflow to run with zero human intervention. Here is the technical flow of the pipeline:

1. **Scheduled Execution:** A GitHub Action triggers a workflow on a set schedule.
2. **Data Retrieval:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3. **Validation:** The system filters out invalid or unavailable observations to ensure data integrity.
4. **Storage:** New observations are appended to a `prices.json` file. I implemented duplicate-date protection to ensure each calendar day has only one record; if FRED has no new observation, the rate is carried forward.
5. **Persistence:** GitHub commits the updated JSON file back to the repository, creating a permanent, versioned historical record.
6. **Visualization:** A lightweight frontend loads this JSON data to render a real-time dashboard.

## Key Features

### Automated Infrastructure
* **Serverless Execution:** No VPS or cloud hosting is required; the entire backend logic lives in GitHub Actions.
* **Git-based Storage:** Historical data is stored directly in the repository, providing an automatic audit trail of every data point collected.

### High-Performance Dashboard
I built a presentation layer using a dark glassmorphism aesthetic that focuses on immediate data clarity:
* **Hero Metrics:** An animated count-up display showing the current index value and a daily change pill.
* **Comparative Analysis:** Instant visibility into 30-day changes and percentage shifts.
* **Historical Context:** Tracking of all-time highs and lows, including the specific dates they occurred and a total "days-tracked" counter.
* **Interactive Visualization:** A Chart.js integration featuring a range selector (1M, 3M, 6M, 1Y, All) for deep-diving into trends.
* **Detailed Logs:** A recent records table equipped with per-day trend tags.

## Data Source and Purpose

It is important to note that this project tracks the **Nominal Broad U.S. Dollar Index** (January 2006 = 100) from the Board of Governors of the Federal Reserve System. This is a broad, trade-weighted measure of the U.S. dollar against the currencies of major trading partners. 

*Note: This is distinct from the ICE U.S. Dollar Index futures contract (DXY).*

## Potential Use Cases

This architecture isn't just for currency tracking. The pattern I've established here—using GitHub Actions as a cron job and a JSON file as a database—can be applied to various scenarios:

* **Price Tracking:** Monitoring e-commerce products or commodity prices over time.
* **API Archiving:** Creating a historical archive for APIs that only provide the "current" state and no historical endpoint.
* **Personal Finance:** Automating the collection of daily exchange rates or stock indices for a personal portfolio dashboard.
* **Lightweight Monitoring:** Tracking public health data, weather patterns, or any daily-updating public metric.