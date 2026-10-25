---
layout: post
title: "us-dollar-index"
date: 2026-10-24 13:10:43 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker Tracking economic indicators often requires expensi..."
---

# Building a Serverless U.S. Dollar Index Tracker

Tracking economic indicators often requires expensive API subscriptions or dedicated server infrastructure to maintain a historical database. I wanted to find a way to collect and visualize the Nominal Broad U.S. Dollar Index without the overhead of managing a backend or paying for hosting.

To solve this, I built the **U.S. Dollar Index Tracker**, an automated, serverless data pipeline that leverages GitHub as both an execution environment and a storage layer.

## What is the U.S. Dollar Index Tracker?

The U.S. Dollar Index Tracker is a lightweight system designed to continuously collect and store the Nominal Broad U.S. Dollar Index (FRED series `DTWEXBGS`). Unlike the common DXY futures contract, this project tracks the trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners, as provided by the Board of Governors of the Federal Reserve System.

My goal was to create a "set it and forget it" system. By combining GitHub Actions with a simple JSON storage file, I've eliminated the need for a dedicated database or a manual update process.

## How the Pipeline Works

I designed the project to run entirely within the GitHub ecosystem. Here is the lifecycle of a data update:

1. **Scheduled Trigger:** A GitHub Action workflow runs on a predefined schedule.
2. **Data Retrieval:** The `fetch-dollar.js` script makes a request to the FRED API to grab the latest observation.
3. **Validation:** The system ignores invalid or unavailable observations to ensure data integrity.
4. **Storage:** New observations are appended to `prices.json`. I've implemented duplicate-date protection to ensure the dataset remains clean, and the system carries the rate forward when FRED has no new observation for a specific day.
5. **Version Control:** GitHub automatically commits the updated `prices.json` back to the repository.
6. **Visualization:** The `index.html` dashboard fetches this JSON file and renders it for the user.

## Key Features

### Serverless Automation
Because the project uses GitHub Actions for execution and the repository for storage, there is no backend to maintain and no server costs.

### Interactive Data Dashboard
I developed a frontend that focuses on clarity and modern aesthetics, featuring:
* **Dark Glassmorphism Design:** A sleek, modern UI with an animated count-up hero section.
* **Real-time Insights:** At-a-glance views of the current index value, daily change pills, and 30-day percentage shifts.
* **Historical Context:** Tracking of all-time highs and lows (with dates) and a total "days-tracked" counter.
* **Advanced Visualization:** An interactive Chart.js graph with selectable time ranges (1 Month, 3 Months, 6 Months, 1 Year, and All).
* **Detailed Logs:** A recent records table featuring per-day trend tags.

### Zero-Build Deployment
The dashboard is designed to work out of the box. There is no frontend build step required; the HTML file interacts directly with the stored data.

## Potential Use Cases

This architecture serves as a blueprint for various data-tracking needs. Beyond tracking the U.S. Dollar Index, this approach can be used for:

* **Economic Monitoring:** Tracking other FRED series or economic indicators that are updated daily or weekly.
* **Personal Finance Tracking:** Automating the collection of asset prices or inflation rates.
* **Portfolio Benchmarking:** Creating a historical record of specific indices to compare against investment performance.
* **Lightweight Archiving:** Any scenario where you need a versioned, time-stamped history of API data without wanting to manage a SQL/NoSQL database.