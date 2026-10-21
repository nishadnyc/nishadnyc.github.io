---
layout: post
title: "us-dollar-index"
date: 2026-10-20 14:11:07 +0000
categories: projects
excerpt: "Building a Serverless Pipeline for the U.S. Dollar Index Tracking financial indices usually require..."
---

# Building a Serverless Pipeline for the U.S. Dollar Index

Tracking financial indices usually requires a dedicated server, a database, and a constant maintenance schedule. I wanted to see if I could strip all of that away. I have developed the **U.S. Dollar Index Tracker**, an automated, serverless data pipeline that continuously collects and stores the Nominal Broad U.S. Dollar Index without requiring a single piece of dedicated infrastructure.

## What is the U.S. Dollar Index Tracker?

This project is a self-sustaining system designed to monitor the **Nominal Broad U.S. Dollar Index (DTWEXBGS)**. Unlike the commonly cited DXY futures contract, this project tracks the broad, trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners, sourced directly from the Board of Governors of the Federal Reserve System via the FRED API.

My primary goal was to leverage GitHub as more than just a code repository. In this project, GitHub serves two critical roles:
1. **The Execution Environment:** Utilizing GitHub Actions to run scheduled data fetches.
2. **The Storage Layer:** Using a JSON file within the repository as a versioned database for historical observations.

## How the Pipeline Works

I designed the system to operate with zero human intervention. The workflow follows a lean, circular process:

*   **Scheduling:** A GitHub Actions workflow triggers on a set schedule.
*   **Fetching:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
*   **Validation:** The system ignores invalid or unavailable observations to ensure data integrity.
*   **Persistence:** New observations are appended to `prices.json`. If FRED has no new observation for a specific day, the rate is carried forward to maintain a consistent daily record.
*   **Commitment:** The updated JSON file is committed back to the repository automatically.
*   **Visualization:** The frontend (`index.html`) pulls the latest data from this JSON file to update the dashboard in real-time.

## Key Features

### Automated Data Management
*   **Serverless Execution:** No backend servers to manage or pay for.
*   **Duplicate Protection:** Built-in logic to prevent duplicate entries for the same calendar date.
*   **Historical Archiving:** Every observation is stored in the repo, creating a permanent, version-controlled history of the index.

### High-Performance Dashboard
I built a lightweight web interface to make the data accessible and actionable. The dashboard features:
*   **Modern UI:** A dark glassmorphism design with an animated count-up hero section.
*   **Instant Analytics:** At-a-glance views of the current index value, daily change (via a change pill), and 30-day percentage shifts.
*   **Historical Milestones:** Automatic tracking of all-time highs and lows, including the specific dates they occurred and a total "days-tracked" counter.
*   **Interactive Visualizations:** Integration with Chart.js allowing users to toggle between 1-month, 3-month, 6-month, 1-year, and all-time views.
*   **Detailed Logs:** A recent records table featuring per-day trend tags for granular analysis.

## Potential Use Cases

Because this project turns a GitHub repository into a living data source, it opens up several interesting use cases:

*   **Financial Monitoring:** For those who need to track the broad strength of the USD without setting up complex API integrations.
*   **Educational Templates:** As a blueprint for how to build other serverless trackers (e.g., tracking other FRED series, crypto prices, or weather data) using GitHub Actions.
*   **Lightweight Reporting:** Providing a public-facing, read-only dashboard for a specific financial metric that doesn't require a traditional hosting provider.

By removing the need for a frontend build step and a dedicated backend, I've created a system that is as easy to maintain as a static website, but as powerful as a scheduled data scraper.