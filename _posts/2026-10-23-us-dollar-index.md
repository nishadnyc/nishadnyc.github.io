---
layout: post
title: "us-dollar-index"
date: 2026-10-23 14:07:27 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker Tracking financial indices usually requires a dedic..."
---

# Building a Serverless U.S. Dollar Index Tracker

Tracking financial indices usually requires a dedicated backend, a database, and a server to handle scheduled API requests. I wanted to challenge that paradigm by building a fully automated, serverless data pipeline that lives entirely within GitHub.

I have developed the **U.S. Dollar Index Tracker**, a system that continuously collects and stores the Nominal Broad U.S. Dollar Index without requiring a single piece of dedicated infrastructure.

## What is the U.S. Dollar Index Tracker?

This project is an automated pipeline designed to monitor the Nominal Broad U.S. Dollar Index (Series: `DTWEXBGS`) sourced from the Federal Reserve Economic Data (FRED) API. 

Unlike the commonly cited DXY (the ICE U.S. Dollar Index futures contract), this project tracks the trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners, as reported by the Board of Governors of the Federal Reserve System.

## The Architecture: GitHub as a Backend

The core philosophy of this project is to utilize GitHub for more than just version control. I have configured GitHub to serve as both the execution environment and the storage layer.

### How the Pipeline Works
1. **Scheduled Automation**: I use GitHub Actions to trigger a workflow on a set schedule.
2. **Data Retrieval**: A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3. **Validation**: The system automatically ignores invalid or unavailable observations to ensure data integrity.
4. **Versioned Storage**: New observations are appended to `prices.json`.
5. **Self-Updating Repository**: GitHub commits the updated JSON file back to the repository automatically.
6. **Presentation**: The `index.html` dashboard reads this JSON file directly to render visualizations.

By using this approach, I've eliminated the need for a database or a running server; the repository itself acts as the database.

## Key Features

### Automated Data Management
- **Zero-Touch Collection**: Once configured, the system runs without human intervention.
- **Historical Continuity**: The system maintains a record of one entry per calendar day. If FRED has no new observation for a specific date, the rate is carried forward.
- **Duplicate Protection**: I've implemented automatic protection to prevent duplicate entries for the same date.

### The Analytics Dashboard
I built a lightweight, frontend-only dashboard that provides a high-level overview of the dollar's movement. Key elements include:
- **Visual Aesthetics**: A dark glassmorphism design featuring an animated count-up hero section.
- **Real-time Metrics**: A current index value display with a daily change pill, 30-day change, and percentage shifts.
- **Historical Context**: Tracking of all-time highs and lows (with dates) and a counter showing the total number of days tracked.
- **Interactive Visualization**: An integrated Chart.js graph with range selectors for 1 month, 3 months, 6 months, 1 year, and the full history.
- **Granular Data**: A recent records table featuring per-day trend tags.

## Potential Use Cases

This architecture serves as a blueprint for any project requiring lightweight, scheduled data collection. Potential applications include:
- **Financial Tracking**: Monitoring specific economic indicators or commodity prices.
- **KPI Monitoring**: Tracking public API metrics for a business or project.
- **Personal Archives**: Creating a permanent, versioned history of a data point that might otherwise be overwritten by an API provider.

By leveraging the synergy between GitHub Actions and JSON storage, I have created a sustainable, free, and low-maintenance tool for financial data observation.