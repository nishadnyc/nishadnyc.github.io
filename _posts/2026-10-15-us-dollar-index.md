---
layout: post
title: "us-dollar-index"
date: 2026-10-15 14:01:45 +0000
categories: projects
excerpt: "Building a Serverless U.S. Dollar Index Tracker Tracking macroeconomic indicators often requires ex..."
---

# Building a Serverless U.S. Dollar Index Tracker

Tracking macroeconomic indicators often requires expensive subscriptions or cumbersome manual data entry. To solve this, I built the U.S. Dollar Index Tracker: an automated, serverless data pipeline designed to continuously collect and store the Nominal Broad U.S. Dollar Index without the need for a dedicated server.

## What is the U.S. Dollar Index Tracker?

My project is a self-sustaining system that monitors the strength of the U.S. dollar against the currencies of its major trading partners. Specifically, it tracks the `DTWEXBGS` series from the Federal Reserve Economic Data (FRED) API. 

Unlike many trackers that follow the ICE U.S. Dollar Index (DXY) futures, I focused on the Nominal Broad U.S. Dollar Index provided by the Board of Governors of the Federal Reserve System. This provides a broad, trade-weighted measure of the dollar's value.

## The Architecture: GitHub as a Backend

The core philosophy behind this project was to eliminate infrastructure overhead. I leveraged GitHub not just for version control, but as both my execution environment and my database.

### How the Pipeline Works
I designed a circular workflow that requires zero human intervention once configured:

1. **Scheduled Execution:** I use GitHub Actions to trigger a workflow on a set schedule.
2. **Data Retrieval:** A Node.js script (`fetch-dollar.js`) requests the latest observation from the FRED API.
3. **Validation:** The system ignores invalid or unavailable observations to ensure data integrity.
4. **Versioned Storage:** New observations are appended to a `prices.json` file.
5. **Automated Commit:** GitHub Actions commits the updated JSON file directly back to the repository, effectively using Git as a historical time-series database.

## Key Features

### Automated Data Management
*   **Serverless Execution:** No backend servers to maintain or pay for.
*   **Duplicate Protection:** Built-in logic to prevent duplicate date entries.
*   **Consistency:** The system ensures one record per calendar day; if FRED has no new observation, the previous rate is carried forward.

### The Visual Dashboard
To make the data accessible, I developed a lightweight web dashboard (`index.html`) that requires no build step. It features:

*   **Glassmorphism UI:** A modern dark-themed interface with an animated count-up hero section.
*   **Real-time Metrics:** A current index value display accompanied by a daily change pill.
*   **Deep Analytics:** Calculation of 30-day changes (nominal and percentage) and all-time high/low tracking with corresponding dates.
*   **Interactive Visualization:** A Chart.js implementation allowing users to toggle between 1-month, 3-month, 6-month, 1-year, and all-time views.
*   **Historical Logs:** A recent records table featuring per-day trend tags.

## Potential Use Cases

This architecture serves as a blueprint for several different types of projects:

*   **Economic Monitoring:** For analysts who need a historical record of Federal Reserve data without paying for premium API endpoints.
*   **Algorithmic Inputs:** The `prices.json` file acts as a public, versioned API that other scripts can fetch to trigger alerts or trades based on dollar strength.
*   **Educational Templates:** This project demonstrates how to use GitHub Actions for ETL (Extract, Transform, Load) processes in a completely free, serverless environment.

## Project Structure

For those interested in the technical layout, here is how I organized the repository:

*   `.github/workflows/update-dollar.yml`: The heartbeat of the project (scheduled automation).
*   `fetch-dollar.js`: The logic for API communication and data storage.
*   `index.html`: The frontend presentation layer.
*   `prices.json`: The historical data store.