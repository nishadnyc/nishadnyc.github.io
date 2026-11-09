---
layout: post
title: "nishadnyc.github.io"
date: 2026-11-08 01:07:52 +0000
categories: projects
excerpt: "Automating My Technical Writing with AI and GitHub Actions Maintaining a personal blog and portfoli..."
---

# Automating My Technical Writing with AI and GitHub Actions

Maintaining a personal blog and portfolio is a challenge for any developer. The friction between writing code and documenting that code often leads to "ghost town" blogs—sites that started with ambition but stopped updating months ago. To solve this, I built a system that turns my active development directly into published content.

## What is this Project?

I have developed an automated content engine integrated into my personal blog and portfolio website. Rather than manually writing every post, I leverage an AI model and GitHub Actions to bridge the gap between my source code and my public-facing portfolio.

The system acts as a bridge that monitors my GitHub repositories, analyzes my work, and auto-generates blog articles that reflect my current technical journey.

## How it Works

The core of the project is a sophisticated automation pipeline. I utilize GitHub Actions as the orchestrator to handle the heavy lifting. 

Here is the technical flow:
*   **Cron Job Scheduling:** I have configured a GitHub workflow that runs on a cron job once every day.
*   **Repository Analysis:** The system scans my GitHub repositories to identify new developments, updates, or projects.
*   **AI Generation:** An AI model processes the repository data to synthesize a coherent, technical blog article.
*   **Automated Deployment:** Once the content is generated, it is pushed to my site, ensuring my portfolio is always up-to-date without manual intervention.

## Key Features

*   **Zero-Touch Publishing:** Once the pipeline is configured, the transition from "code commit" to "blog post" happens automatically.
*   **AI-Driven Synthesis:** The project doesn't just list changes; it uses an AI model to generate meaningful articles based on the context of my repositories.
*   **Daily Automation:** By running a daily cron job, the site remains fresh and reflects my real-time productivity.
*   **Integrated Portfolio:** The system is built directly into my personal website, serving as both a professional landing page and a dynamic technical log.

## Potential Use Cases

While I use this for my personal portfolio, this architecture opens up several possibilities for other developers and teams:

*   **Automated Changelogs:** Transforming raw commit histories into human-readable updates for stakeholders.
*   **Developer Portfolios:** Ensuring that a portfolio always reflects the most recent projects without requiring the developer to stop coding to write documentation.
*   **Internal Knowledge Bases:** Automatically generating summaries of project progress for internal engineering teams.
*   **Technical Documentation:** Creating a first-draft narrative of how a project has evolved over time.