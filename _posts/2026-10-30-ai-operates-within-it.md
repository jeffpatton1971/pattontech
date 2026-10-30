---
layout: single
title: "Engineering Intent: AI Operates Within It"
date: 2026-10-30 08:00:00 -0500
categories:
  - blog
tags:
  - October 2026
  - SoftwareEngineering
  - AIAssistedDevelopment
  - SoftwareArchitecture
  - Engineering Intent
author: Jeff Patton
comments: true
published: true
---

Over the past few weeks, I've been writing about an approach to AI-assisted development that's gradually evolved through my work on BAT and some of my personal projects.

It started with a fairly simple observation. AI was becoming increasingly capable of helping me write software, but I was spending a surprising amount of time keeping it aligned with what we were trying to accomplish.

That led to a lot of experimentation with conversations, sprint plans, handoffs, documentation, and eventually a governance structure that could exist independently of any particular AI tool.

In the previous articles, I've explored three principles:

**Engineering intent is authoritative.**

**Documentation preserves it.**

**Governance protects it.**

Which brings me to the fourth.

**AI operates within it.**

## Giving AI Room to Work

One thing I've been pretty consistent about is that I don't want AI to simply do whatever I tell it.

If I'm making a questionable decision, I want it to say something. If there's a better approach, I want to hear about it.

That might sound contradictory when I'm also talking about establishing rules and constraints that AI agents are expected to follow.

But I don't really see it that way.

When I work with another engineer, I don't expect them to blindly follow every instruction. I expect them to understand what we're trying to accomplish, consider the existing architecture, and use their judgment.

If something doesn't make sense, I'd rather they question it.

I want much the same thing from AI.

The difference is that an AI agent doesn't necessarily have the benefit of years of experience working with a particular codebase, attending design discussions, or understanding why certain decisions were made.

It needs a way to discover that information.

And I'd prefer not to have to explain everything from scratch every time I start a new session.

## What Happens When an Agent Enters a Repository?

This became particularly interesting as BAT grew into a collection of repositories.

For a long time, I was the one providing context. I knew how the different components fit together, which decisions we'd made, and where things were supposed to go.

If ChatGPT suggested something that didn't fit, I could usually recognize it and steer the conversation back.

But that wasn't going to scale very well.

What if I wanted to use another AI agent? What if another developer wanted to work on the project using Copilot, Claude, or something running locally?

I started thinking about what that experience should look like.

Ideally, someone should be able to open any BAT repository and have their development tools discover the expectations that govern the work.

That led us to files like `AGENTS.md`, Copilot instructions, and other agent-specific guidance.

Those files aren't intended to contain the entire architecture. They're more like entry points.

They establish where to look, which conventions apply, and how to find the central documentation, architectural decisions, and project constitution.

The idea is that the agent shouldn't need to know the history of every conversation we've ever had. It should be able to discover the decisions that matter to the work it's doing.

Of course, writing an instruction file doesn't guarantee that an agent will follow it. Different tools handle instructions differently, and even a well-informed agent can make mistakes.

But at least we're no longer relying entirely on the conversation to establish the rules.

## Knowing When to Stop

I think one of the more interesting challenges with AI agents is figuring out how much independence to give them.

There are plenty of situations where I want an agent to work through a problem, make reasonable implementation choices, write tests, and report back on what it accomplished.

I don't need to approve every variable name or review every intermediate step.

But there are other situations where I'd prefer it to stop and ask a question.

Changing a public interface, introducing a new architectural dependency, or moving responsibilities between components might all have implications beyond the immediate task.

An agent might have a perfectly good reason for suggesting one of those changes.

I'd just rather have that conversation before the change becomes part of the implementation.

And that brings me back to the distinction between disagreement and drift.

I want agents to challenge architectural decisions. I don't want them silently replacing those decisions while working on something else.

The same expectations should apply to me, or anyone else contributing to the project.

## Can We Actually Verify Any of This?

There's another problem I've become increasingly interested in.

How do we know the implementation actually follows the documented architecture?

An AI agent can tell me it followed the instructions. It can provide a summary of what it changed, explain why, and report that the tests passed.

That's useful information.

But it isn't necessarily evidence that the implementation respects every architectural boundary.

I've seen enough AI-generated code to know that a working implementation and an appropriate implementation aren't always the same thing.

Some expectations can be verified automatically. We can test behavior, validate contracts, enforce dependency boundaries, and inspect changes to public interfaces.

Others still require engineering judgment.

I'm interested in making more of those expectations explicit so we have something concrete to review against.

This is one of the areas where I think there's still quite a bit of work to do.

## The Model Shouldn't Define the Project

More recently, I've been exploring what it would take to move more of my AI-assisted development onto local hardware.

That introduces another set of challenges, particularly when working with smaller models that may not have the same capabilities or context capacity as the larger hosted models.

But it also reinforces something I've been thinking about for a while.

Why should the engineering knowledge of a project depend on the model I'm using?

If I switch from ChatGPT to Claude, or from a hosted model to something running through Ollama, I shouldn't have to reconstruct the architecture from scratch.

The tools may need different instructions. They may have different capabilities, and some may need more guidance than others.

But the underlying decisions about what we're building and why shouldn't change.

This thinking is part of what led me toward [llamar42](https://github.com/llamarc42), a project focused on establishing a consistent documentation and governance framework.

And it's influenced how I've approached [InfrastructureIntent](https://github.com/InfrastructureIntent), where we've worked through architectural decisions and project boundaries before getting too far into implementation.

I'm still figuring out what that framework should look like, and I don't expect every project to need the same amount of structure.

But I like the idea that the project owns its engineering knowledge, rather than leaving that knowledge scattered across AI conversations or tied to a particular development tool.

## Bringing It Together

Looking back at how I've worked with AI over the past few years, I don't think there was any single moment when all of this fell into place.

It was a gradual process of running into problems, trying something different, and keeping the things that seemed to help.

Long conversations worked until they became unwieldy.

Sprint plans and handoffs helped preserve continuity, but didn't always capture why we'd made particular decisions.

Documentation gave those decisions somewhere to live.

Governance gave us a way to establish expectations around them.

And now I'm exploring how different AI agents can work effectively within that structure.

I don't think this solves every problem with AI-assisted development. Agents will still misunderstand instructions, suggest questionable approaches, and occasionally produce code that doesn't belong.

I'll undoubtedly make some of those mistakes myself.

What I'm trying to build is an environment where those problems are easier to recognize and correct, without losing the ability to explore ideas or challenge the decisions we've already made.

That brings me back to the four principles that started this series.

**Engineering intent is authoritative.**

**Documentation preserves it.**

**Governance protects it.**

**AI operates within it.**

I didn't start out trying to define a methodology for AI-assisted software development. I was just trying to find a better way to work.

Somewhere along the way, those experiments started becoming a repeatable approach.

I'm interested to see where it goes from here.
