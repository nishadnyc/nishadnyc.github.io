---
layout: post
title: "us-dollar-index"
date: 2026-09-15 07:14:57 +0000
categories: projects
excerpt: "Tracking the U.S. Dollar Index with a Serverless Pipeline Tracking macroeconomic indicators often r..."
---

# Tracking the U.S. Dollar Index with a Serverless Pipeline

Tracking macroeconomic indicators often requires either expensive subscriptions or the tedious task of manual data entry. To solve this, I built the **U.S. Dollar Index Tracker**, an automated, serverless data pipeline designed to continuously collect and store the Nominal Broad U.S. Dollar Index without the need for a dedicated server.

## What is the U.S. Dollar Index Tracker?

The project is a self-sustaining system that monitors the **Nominal Broad U.S. Dollar Index** (FRED series `DTWEXBGS`). Unlike the common DXY futures contract, this specific index is a trade-weighted measure of the U.S. dollar against the currencies of major U.S. trading partners, provided by the Board of Governors of the Federal Reserve System.

My goal was to create a system where GitHub serves as both the execution environment and the versioned storage layer. By leveraging GitHub Actions and a simple JSON flat-file database, I have eliminated the need for a traditional backend or a managed database.

## How the Pipeline Works

The project operates as a closed loop that requires zero human intervention once configured. Here is the technical flow:

1.  **Scheduled Trigger:** A GitHub Actions workflow runs on a predefined schedule.
2.  **Data Retrieval:** The `fetch-dollar.js` script makes a request to the FRED API to retrieve the latest observation.
3.  **Validation:** The system filters out invalid or unavailable observations to ensure data integrity.
4.  **Storage:** New observations are appended to `prices.json`. I have implemented duplicate-date protection to prevent redundant entries.
5.  **Version Control:** The workflow automatically commits the updated `prices.json` back to the repository.
6.  **Visualization:** The `index.html` dashboard reads the JSON file and renders the data using Chart.js.

## Key Features

I focused on making the tool lightweight and informative. The primary features include:

*   **Serverless Architecture:** No dedicated backend or hosting costs; the entire pipeline runs via GitHub Actions.
*   **Automated Data Collection:** Continuous updates ensure the historical record stays current.
*   **Comprehensive Analytics:** The dashboard provides more than just a chart; it calculates:
    *   Current index value and daily change.
    *   30-day change and percentage change.
    *   Historical high and low values.
*   **Interactive Visualization:** An integrated Chart.js interface allows for easy exploration of trends.
*   **Zero-Build Frontend:** The dashboard is a pure HTML/JS implementation, meaning no complex build steps are required to deploy or update the UI.

## Potential Use Cases

This architecture provides a blueprint for anyone looking to track time-series data without managing infrastructure. Some potential applications include:

*   **Macroeconomic Monitoring:** Following currency strength to inform trading or investment strategies.
*   **Academic Research:** Creating a version-controlled historical archive of specific economic indicators.
*   **Custom Dashboards:** Using the same logic to track other FRED series or public API data points (e.g., inflation rates, unemployment data) to create a personalized economic cockpit.

## Project Structure

The repository is organized to keep the automation logic separate from the presentation layer:

*   `.github/workflows/update-dollar.yml`: The engine that handles the scheduling and automation.
*   `fetch-dollar.js`: The logic for API interaction and data persistence.
*   `index.html`: The frontend dashboard interface.
*   `prices.json`: The historical data store.