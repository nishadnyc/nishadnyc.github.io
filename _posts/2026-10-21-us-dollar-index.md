---
layout: post
title: "us-dollar-index"
date: 2026-10-21 14:23:36 +0000
categories: projects
excerpt: "Building a Serverless Pipeline for the U.S. Dollar Index Tracker Tracking financial indices often r..."
---

# Building a Serverless Pipeline for the U.S. Dollar Index Tracker

Tracking financial indices often requires expensive subscriptions, dedicated servers, or tedious manual data entry. To solve this, I built the **U.S. Dollar Index Tracker**, an automated, serverless data pipeline designed to continuously collect and store the Nominal Broad U.S. Dollar Index without the need for a traditional backend.

## What is the U.S. Dollar Index Tracker?

My project is a self-sustaining system that monitors the strength of the U.S. dollar against the currencies of its major trading partners. Specifically, it tracks the **Nominal Broad U.S. Dollar Index (Series: DTWEXBGS)** provided by the Federal Reserve Economic Data (FRED) API. 

Unlike the common DXY futures contract, this index provides a broad, trade-weighted measure of the dollar, offering a comprehensive view of its global standing.

## The Architecture: Turning GitHub into a Database

The core philosophy behind this project was to eliminate infrastructure overhead. I wanted a system that ran itself with zero human intervention. To achieve this, I leveraged GitHub as more than just a code repository; I used it as both my execution environment and my storage layer.

### How the Pipeline Works
1. **Scheduled Automation**: I use GitHub Actions to trigger a workflow on a regular schedule.
2. **Data Acquisition**: A Node.js script (`fetch-dollar.js`) requests the latest daily observation from the FRED API.
3. **Data Validation**: The system automatically ignores invalid or unavailable observations to ensure data integrity.
4. **Versioned Storage**: New observations are appended to a `prices.json` file. GitHub then commits this updated file back to the repository, creating a permanent, versioned historical record.
5. **Frontend Presentation**: A lightweight HTML dashboard reads the JSON file and renders the data for the user.

## Key Features

### 🛠️ Automated Backend
* **Zero-Server Footprint**: By using GitHub Actions, I've eliminated the need for a dedicated VPS or cloud hosting for the data collection process.
* **Smart Data Handling**: The pipeline includes duplicate-date protection and a carry-forward mechanism that maintains one record per calendar day, even when FRED has no new observation.

### 📊 Modern Data Dashboard
I designed a high-performance frontend that requires no build step. The dashboard features:
* **Glassmorphism UI**: A sleek, dark-themed interface with an animated count-up hero section.
* **Real-time Metrics**: Instant visibility into the current index value, daily change "pills," and the 30-day percentage change.
* **Historical Insights**: Automatic calculation of all-time highs and lows (including dates) and a total days-tracked counter.
* **Interactive Visualizations**: Powered by Chart.js, the dashboard allows users to toggle between 1-month, 3-month, 6-month, 1-year, and all-time views.
* **Detailed Logs**: A recent records table with per-day trend tags for granular analysis.

## Potential Use Cases

This project serves as a blueprint for anyone looking to build lightweight financial monitors. Potential applications include:

* **Economic Research**: Maintaining a clean, accessible dataset of the Broad USD Index for academic or professional analysis.
* **Trading Signal Monitoring**: Using the dashboard to quickly gauge the long-term trend of the US Dollar before making currency-related trades.
* **Portfolio Correlation**: Comparing the movement of the USD Index against other asset classes (like gold or equities) using the stored historical data.
* **Learning Serverless Patterns**: Serving as a reference for how to use GitHub Actions and JSON files to create a "database-less" application.