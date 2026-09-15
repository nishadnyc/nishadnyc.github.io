---
layout: post
title: "us-dollar-index"
date: 2026-09-14 18:04:22 +0000
categories: projects
excerpt: "Tracking the Strength of the Greenback: My US Dollar Index Dashboard Monitoring the value of the U...."
---

# Tracking the Strength of the Greenback: My US Dollar Index Dashboard

Monitoring the value of the U.S. Dollar is essential for anyone interested in global economics, trade, or currency markets. To simplify this process, I built a lightweight dashboard that tracks the **Nominal Broad U.S. Dollar Index** in real-time, providing a clear visual representation of the dollar's strength against a basket of currencies from major trading partners.

You can explore the live tool here: [View the dashboard](https://nishad.top/us-dollar-index/)

## What is the Nominal Broad U.S. Dollar Index?

Unlike the common DXY (ICE U.S. Dollar Index) futures contract, my project tracks the **DTWEXBGS** series from the Federal Reserve Economic Data (FRED). This is a broad, trade-weighted measure provided by the Board of Governors of the Federal Reserve System.

The index uses January 2006 as its base (100), offering a standardized way to observe how the dollar fluctuates daily against the currencies of the United States' primary trading partners.

## Key Features

I designed this dashboard to be efficient and automated, ensuring the data is always current without requiring manual updates.

*   **Automated Data Acquisition:** The system automatically fetches the latest daily index values from the FRED API.
*   **Smart Data Management:** Observations are stored in a `prices.json` file. I implemented logic to automatically ignore invalid data points and prevent duplicate dates.
*   **Real-Time Metrics:** Beyond just the current value, the dashboard calculates:
    *   The daily change.
    *   The 30-day change and the corresponding percentage shift.
    *   The historical high and low peaks.
*   **Interactive Visualization:** Using Chart.js, I’ve integrated a responsive line chart that allows users to visualize historical trends at a glance.
*   **Zero-Build Architecture:** The frontend is designed to run without a complex build step, making it incredibly fast to deploy and maintain.
*   **CI/CD Integration:** I utilize GitHub Actions to automate the update process, ensuring the data remains fresh.

## How the System Works

The architecture is split into a data-fetching layer and a presentation layer:

1.  **The Fetcher:** I wrote `fetch-dollar.js` to handle the API communication. It requests the latest observations from FRED, filters out invalid entries (marked as `.`), and appends new, valid observations to the `prices.json` database.
2.  **The Dashboard:** The `index.html` file acts as the primary interface. It loads the historical data directly from `prices.json` and uses Chart.js to render the metrics and the interactive graph.

## Potential Use Cases

This tool is particularly useful for several different profiles:

*   **Economists and Analysts:** Quickly gauging the broad strength of the USD without navigating complex government databases.
*   **International Traders:** Understanding how a strong or weak dollar might impact the pricing of imports and exports.
*   **Developers:** As a reference for how to build a lightweight, serverless-style dashboard using GitHub Actions and a JSON-based data store.