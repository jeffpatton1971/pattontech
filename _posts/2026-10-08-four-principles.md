---
layout: single
title:  "Engineering Intent: Four Principles for AI-Assisted Development"
date: 2026-10-08 07:00:00 -0500
categories: blog
tags:
    - October
    - "2026"
    - SoftwareEngineering
    - AIAssistedDevelopment
    - SoftwareArchitecture
    - Engineering Intent
author: Jeff
comments: true
published: true
---

I've spent a lot of time over the past few years experimenting with AI-assisted software development, and one thing I've noticed is that the more capable the tools become, the more important it is to be deliberate about how we use them.

Somewhere along the way, I started approaching projects a little differently.

Instead of starting with code, I often start with documentation. Not just a README, but the architectural principles, design decisions, constraints, and expectations that establish how a project should evolve.

That approach grew out of work I was doing on a large, multi-repository build automation platform. I've since carried many of those ideas into my own projects, including [InfrastructureIntent](https://github.com/InfrastructureIntent).

Recently, I found myself trying to summarize the philosophy behind it all:

**Engineering intent is authoritative.**
**Documentation preserves it.**
**Governance protects it.**
**AI operates within it.**

I like that because it puts engineering decisions at the center of the development process, regardless of whether the work is being done by a person, an AI agent, or some combination of the two.

It doesn't mean documenting everything before writing a line of code. It means establishing enough shared understanding that the architecture doesn't get reinvented every time a new development session starts.

I'm still refining this approach, and it's becoming the foundation of a project I've been working on called [llamarc42](https://github.com/llamarc42).

Over the next few weeks, I'd like to explore each of those ideas, share what I've learned, and hopefully hear how other developers are approaching the same challenges.

I'm particularly curious: **how are you preserving architectural intent when working with AI coding agents?**
