---
layout: post
title: "nishadnyc.github.io"
date: 2026-10-11 00:42:05 +0000
categories: projects
excerpt: "Automating My Digital Presence: An AI-Powered Portfolio and Blog Maintaining a personal portfolio i..."
---

# Automating My Digital Presence: An AI-Powered Portfolio and Blog

Maintaining a personal portfolio is often a struggle between the desire to share my work and the time required to actually write about it. To solve this, I have built a self-sustaining ecosystem for my personal blog and portfolio website that bridges the gap between my code and my content.

## What is this Project?

My portfolio is more than just a static site; it is an automated content engine. I have integrated an AI-driven pipeline that monitors my GitHub repositories and automatically generates blog articles based on the code I write. By leveraging GitHub Actions and cron jobs, the site essentially documents my technical journey in real-time.

## The Purpose: From Code to Content

The primary goal of this project is to eliminate the "blank page" problem. As a developer, I spend most of my time in the IDE, not in a CMS. I wanted a system where my active development naturally translates into public-facing content. 

By automating the generation of blog posts, I ensure that my portfolio remains current and that my latest projects are highlighted without requiring a manual writing phase for every single update.

## Key Features

To achieve this automation, I implemented several critical components:

*   **AI-Driven Article Generation:** I utilize an AI model to analyze my GitHub repositories and synthesize the technical details into readable blog articles.
*   **Scheduled Automation:** Using GitHub Workflows, I have configured a cron job that triggers once every day. This ensures the site is consistently updated with new insights and project milestones.
*   **Integrated CI/CD Pipeline:** The project utilizes GitHub Pages for deployment, ensuring that once the AI generates the content, it is seamlessly deployed to the live site.
*   **Automated Workflow Badges:** I track the health of my deployment and generation pipelines directly via GitHub Action badges:
    *   ![pages-build-deployment](https://github.com/nishadnyc/nishadnyc.github.io/actions/workflows/pages/pages-build-deployment/badge.svg)
    *   ![Daily Generate Blog Content](https://github.com/nishadnyc/nishadnyc.github.io/actions/workflows/daily-blog-generation.yml/badge.svg)

## Potential Use Cases

While I currently use this for my personal brand, this architecture opens up several possibilities for other developers and organizations:

*   **Automated Project Documentation:** Transforming commit histories and READMEs into a series of "Dev Logs" for stakeholders.
*   **Developer Portfolios:** Creating a "living" portfolio that proves active coding habits to recruiters without manual upkeep.
*   **Technical Knowledge Bases:** Automatically summarizing repository changes into a searchable internal blog for engineering teams.