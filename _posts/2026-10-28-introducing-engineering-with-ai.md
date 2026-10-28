---
layout: single
title: "Introducing Engineering with AI: A Practical Prompt Series"
date: 2026-10-28
categories:
  - blog
tags:
  - AI
  - Prompt Engineering
  - Software Engineering
  - Engineering Intent
  - AI-Assisted Development
  - Engineering with AI
author: Jeff Patton
comments: true
published: true
---
I came across a post on LinkedIn recently offering twelve prompts for leaders who want to make better use of AI. The prompts were part of [Matt Shumer's AI Work Toolkit](https://somethingbig.ai/prompts-linkedin), a collection covering everything from challenging assumptions and researching decisions to writing, negotiating, and building small tools.

I was curious enough to take a look.

What interested me wasn't necessarily the prompts themselves, although several were quite good. It was the thought that went into them. Rather than simply telling an AI to act as an expert, they established expectations, provided context, defined what the output should look like, and, in several cases, explicitly instructed the model to challenge assumptions rather than agree with the user.

That got me thinking about the way I've been using AI in software engineering.

Over the past several years, I've spent a considerable amount of time working with AI on software development, architecture, automation, documentation, and testing. Some of that work has involved building systems from scratch. Other times, I've been trying to understand unexpected behavior, evaluate an architectural decision, or work through an idea that wasn't particularly well defined when I started.

I've learned that the way I approach these conversations can make a considerable difference in the results.

Sometimes I need AI to help me understand a problem before we start designing a solution. Sometimes I need it to challenge my assumptions or identify something I've overlooked. Other times, I need it to review an implementation against established requirements without being influenced by the fact that it helped write the code in the first place.

Those are different problems, and they deserve different approaches.

## Why an Engineering Prompt Series?

I've written previously about engineering intent, documentation, and the importance of preserving decisions when working with AI agents. Much of that discussion has focused on making engineering work understandable and maintainable, particularly when multiple people or agents are contributing to the same project.

I think there's a practical extension to those ideas.

If we're going to rely on AI to help design, implement, and maintain software, we should put some thought into how we ask it to work with us.

A prompt can do more than describe a task. It can establish boundaries, require evidence, define expectations, and specify when the model should stop and ask questions rather than make assumptions.

That doesn't guarantee a correct answer. A well-written prompt won't compensate for a model that lacks the capability to perform the task, and even capable models can produce convincing results that turn out to be wrong.

Still, I think we can do better than repeatedly asking an AI to review something, make it better, or implement a feature without establishing what success actually means.

The idea behind this series is to explore how.

## How the Series Will Work

I'm planning to publish one prompt each day.

Each article will focus on a specific engineering problem or activity. I'll explain why I think the prompt is useful, how I arrived at its structure, and what I'm trying to accomplish with it.

I'll also include the complete prompt so readers can copy it and try it themselves.

Where possible, I'll demonstrate the prompt using an actual engineering scenario. Sometimes that might involve architecture or implementation planning. Other times, it could be a code review, a testing problem, or an attempt to understand why something isn't behaving as expected.

I don't want these examples to be demonstrations where everything conveniently works. If a prompt produces a poor result, makes questionable assumptions, or needs revision, that's worth discussing too.

The prompts will generally fall into four groups.

### Discover & Challenge

Before solving a problem, we need to understand it.

These prompts will focus on exploring requirements, identifying assumptions, questioning proposed solutions, and separating what we know from what we think we know.

### Design & Decide

Once we understand the problem, we need to make decisions about how to approach it.

These prompts will explore architectural boundaries, responsibilities, tradeoffs, implementation planning, and how to capture decisions so they remain understandable later.

### Validate & Preserve

Getting something to work isn't necessarily the same as getting it right.

This group will focus on evaluating implementations, challenging test results, reviewing work independently, and preserving the reasoning behind what we've built.

### Communicate

Engineering involves explaining decisions as much as making them.

These prompts will focus on technical writing, documentation, design discussions, and communicating complex ideas without losing the details that matter.

These groups are a starting point, not a rigid framework. I expect additional topics to emerge as the series develops.

## A Prompt Is a Starting Point

One thing I want to avoid is presenting these prompts as magic instructions that somehow make AI reliable.

I don't think that's how this works.

The model matters. The context matters. The quality of the information we provide matters. A prompt that works well with one model may behave differently with another, particularly when the task involves complex reasoning, tool use, or working across a large codebase.

I also don't believe every engineering interaction needs an elaborate prompt. Sometimes a straightforward question is exactly what's needed. Adding structure only makes sense when that structure improves the result.

What I'm interested in is finding out which instructions actually help, why they help, and where they introduce unnecessary complexity.

I'm approaching this as an ongoing exercise, not a finished methodology.

## Where We'll Start

The first prompt will focus on something I've encountered repeatedly when working with AI: the tendency to start solving a problem before we've established what the problem actually is.

It's easy to describe an issue and receive an immediate implementation plan. Sometimes that works. Other times, we end up spending considerable effort building something before realizing that we misunderstood the requirements or overlooked a simpler approach.

So the first prompt will be **Understand the Problem Before Solving It**.

From there, we'll look at challenging assumptions, establishing what is authoritative, making architectural decisions, and eventually validating the work we've produced.

I'm interested in seeing how these prompts hold up in practice, including across different AI models.

Matt Shumer's collection was the inspiration for taking a closer look at this. What follows will be my own exploration of how prompting fits into the engineering process, drawing on the projects I've worked on and the problems I've encountered along the way.

We'll see what works.
