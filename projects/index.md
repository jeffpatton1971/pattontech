---
layout: single
title: Projects
permalink: /projects/
description: "Engineering work in infrastructure automation, .NET developer tools, and durable context for AI-assisted development."
excerpt: "From PowerShell and Azure automation to infrastructure architecture, .NET tools, and AI-assisted engineering."
classes: wide
portfolio: true
---

<div class="portfolio-intro" markdown="1">
I've spent a good portion of my career building tools to make infrastructure and software development more consistent, repeatable, and easier to manage.

A lot of that started with PowerShell and Azure automation. Over time, the work grew into larger C# systems and infrastructure automation platforms. More recently, I've been exploring how AI can participate in engineering while keeping the decisions and requirements behind the code intact.

These projects show that progression: the problems I worked on, the boundaries I chose, and what those decisions mean for the people using the tools.
</div>

<nav class="portfolio-nav" aria-label="Projects on this page">
  <a href="#featured-projects">Featured projects</a>
  <a href="#earlier-projects">Earlier projects</a>
  <a href="{{ '/series/engineering-intent/' | relative_url }}">Engineering Intent articles</a>
</nav>

## Featured Projects

BAT, InfrastructureIntent, and llamarc42 connect through my engineering experience and interests. Each has its own purpose and ownership boundaries.

{% assign featured = site.projects | where: "portfolio_group", "featured" | sort: "order" %}
<div class="project-grid">
  {% for project in featured %}
    {% include project-card.html project=project %}
  {% endfor %}
</div>

## Earlier Projects

PowerShell modules and shared libraries were an important part of how I moved from automating individual tasks to building reusable tools.

{% assign earlier = site.projects | where: "portfolio_group", "earlier" | sort: "order" %}
<div class="project-grid project-grid--earlier">
  {% for project in earlier %}
    {% include project-card.html project=project %}
  {% endfor %}
</div>

More of the background is on my [About page]({{ '/about/' | relative_url }}). I write about the reasoning behind this work in the [Engineering Intent series]({{ '/series/engineering-intent/' | relative_url }}).
