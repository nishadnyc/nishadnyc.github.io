---
layout: post
title: "us-dollar-index"
date: 2026-10-19 22:12:53 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker Tracking financial indicators usually requires a de..."
---

# Building a Serverless U.S. Dollar Index Tracker

Tracking financial indicators usually requires a dedicated server, a database, and a complex backend. However, I wanted to see if I could build a fully automated, self-sustaining data pipeline using nothing but GitHub. I have developed the **U.S. Dollar Index Tracker**, a serverless system that continuously collects, stores, and visualizes the Nominal Broad U.S. Dollar Index.

## What is this Project?

The U.S. Dollar Index Tracker is an automated data pipeline designed to monitor the Nominal Broad U.S. Dollar Index (Series: `DTWEXBGS`) from the Federal Reserve Economic Data (FRED) API. 

Unlike many trackers that follow the ICE U.S. Dollar Index (DXY), my project focuses on the trade-weighted measure provided by the Board of Governors of the Federal Reserve System. This index offers a broad view of the U.S. dollar's strength against the currencies of major trading partners.

## The Architecture: Serverless by Design

My primary goal was to eliminate the need for a dedicated backend. I achieved this by leveraging GitHub for two distinct purposes: as an execution environment and as a versioned storage layer.

### How the Pipeline Works
1. **Scheduled Automation:** I use GitHub Actions to trigger a workflow on a regular schedule.
2. **Data Fetching:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3. **Validation:** The system ignores invalid or unavailable observations to ensure data integrity.
4. **Versioned Storage:** New observations are appended to `prices.json`. GitHub then commits these updates directly back to the repository.
5. **Frontend Delivery:** An `index.html` file serves as a lightweight dashboard, loading the JSON data and rendering it via Chart.js.

This cycle requires zero human intervention once configured.

## Key Features

### Automated Data Management
- **Hands-off Collection:** GitHub Actions handles all updates automatically.
- **Consistent Records:** The system ensures one record per calendar day; if FRED has no new observation, the previous rate is carried forward.
- **Duplicate Protection:** I have implemented automatic duplicate-date protection to keep the dataset clean.

### High-Performance Dashboard
I built a presentation layer that focuses on clarity and modern aesthetics:
- **Glassmorphism UI:** A dark-themed, glassmorphism dashboard featuring an animated count-up hero section.
- **Real-time Insights:** The dashboard displays the current index value, daily change pills, and 30-day percentage shifts.
- **Historical Context:** I've included all-time high/low values with their corresponding dates and a counter for the total days tracked.
- **Interactive Visualization:** Using Chart.js, users can toggle between different timeframes (1 Month, 3 Months, 6 Months, 1 Year, or All) to analyze trends.
- **Detailed Logging:** A recent records table provides a granular look at daily movements with trend tags.

## Potential Use Cases

This architecture isn't just for tracking currency indices. The "GitHub-as-a-Database" pattern I used here can be applied to various scenarios:

- **Price Tracking:** Monitoring the cost of specific goods or services over time from public APIs.
- **Public Health Monitoring:** Automating the collection of daily health statistics.
- **Environmental Tracking:** Logging weather or pollution data for a specific region.
- **Portfolio Benchmarking:** Creating a simple, public historical record of a specific asset's performance without paying for hosting.

By combining GitHub Actions with a simple JSON flat-file, I've created a system that is free to host, easy to maintain, and completely transparent.