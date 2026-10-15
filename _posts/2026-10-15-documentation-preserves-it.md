---
layout: single
title:  "Engineering Intent: Documentation Preserves It"
date: 2026-10-15 07:00:00 -0500
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

## Documentation Preserves It

In my last post, I talked about how my approach to AI-assisted development evolved from relying on long conversations to something a little more structured.

For a long time, the conversation was where everything happened. We'd discuss an idea, explore different approaches, make a decision, and move on.

That worked pretty well, at least until the conversation got too large or we needed to start a new one.

Eventually, I realized that remembering what we were working on wasn't quite the same as remembering why we were doing it that way.

And that turned out to be a much bigger problem.

## We've Already Had This Conversation

I can't tell you how many times I've found myself revisiting a decision that I was pretty sure we'd already made.

Sometimes the AI would suggest a different approach. Sometimes I'd question something we'd implemented weeks earlier. Other times we'd find ourselves discussing an architectural problem that felt strangely familiar.

And occasionally, it turned out we'd already solved it.

The frustrating part wasn't necessarily having the conversation again. I've always felt that revisiting a decision can be useful, especially when new information becomes available.

The problem was not having a reliable way to distinguish between a new discussion and one we'd already worked through.

Why had we chosen that approach? What alternatives had we considered? Were there constraints that made another solution impractical?

Without that information, it was surprisingly easy to start making the same decisions all over again.

Or worse, make a different decision without realizing we'd just contradicted the original one.

## Documentation Started Becoming Part of the Work

As BAT grew into a collection of repositories, we started putting more effort into documentation.

Initially, a lot of that was fairly conventional. Architecture diagrams, development guides, descriptions of how the various components worked together.

But over time, the documentation started taking on a different role.

We began recording architectural decisions, including the reasoning behind them. We established project-wide principles and documented the boundaries between components.

Some of those decisions became Architecture Decision Records, or ADRs.

I particularly like ADRs because they don't need to be complicated. They provide a way to say, essentially, here's the problem we were trying to solve, here's what we decided, and here's why.

That last part matters.

Knowing that something was designed a particular way is useful. Knowing why it was designed that way is considerably more valuable when you're thinking about changing it.

And since those decisions lived in the repositories, they weren't dependent on me remembering them or an AI conversation retaining them.

## Not Every Document Has the Same Job

Another thing I started recognizing was that documentation serves different purposes.

A sprint plan tells us what we're trying to accomplish.

A handoff tells us where we left off.

A development guide explains how we expect work to be done.

An ADR explains why an important decision was made.

And a project constitution establishes principles that should guide the work regardless of which repository or developer happens to be involved.

Those aren't interchangeable.

For a while, I think I was trying to solve several different problems by simply giving the AI more context.

But more context isn't necessarily better context.

A detailed history of everything we've discussed might contain the answer, but it also contains abandoned ideas, outdated assumptions, and decisions that have since changed.

What I really needed was a way to separate the information that mattered today from the history of how we got there.

That distinction has become increasingly important as I've started experimenting with different AI agents and local models.

## Making the Documentation Discoverable

One of the things I'm particularly pleased with in BAT is how we've approached documentation across repositories.

Rather than trying to duplicate all of the architectural knowledge everywhere, we established central documentation and gave individual repositories a way to point back to it.

Files like `AGENTS.md`, Copilot instructions, and other agent-specific guidance help establish where an agent should look for the rules governing its work.

The goal is that someone can start working in a repository they've never seen before and find their way back to the decisions, constraints, and conventions that matter.

And I don't mean just AI agents.

A developer joining the project should have access to the same information.

I think that's an important distinction. The documentation isn't supposed to be a collection of secret instructions for AI. It's supposed to represent the shared understanding of how the project works and how we expect it to evolve.

Different tools may need different instructions to discover that information, but the underlying engineering decisions shouldn't change depending on which tool you're using.

## Starting With Documentation

More recently, I started approaching this from the other direction.

With [InfrastructureIntent](https://github.com/InfrastructureIntent), we spent a considerable amount of time working through the documentation before getting very far into implementation.

We explored the architecture, challenged assumptions, defined boundaries, and recorded decisions.

And interestingly, the project itself evolved during that process.

What started as an idea for a reusable engine became something with a clearer purpose and a different identity.

That wasn't documentation slowing development down. It was documentation giving us a place to work through the design before committing too heavily to an implementation.

I'm not suggesting every project needs an extensive collection of documents before anyone writes code. That sounds like a good way to spend a lot of time documenting software that might never exist.

But I am finding value in establishing some of those decisions early and allowing the documentation to evolve alongside the implementation.

## Where This Is Leading

A lot of this thinking has influenced another project I've been working on called [llamar42](https://github.com/llamarc42).

The idea is to take some of what I've learned through BAT and InfrastructureIntent and develop a reusable framework for organizing and maintaining project documentation.

Not another AI coding assistant, but a way to establish the information that developers and AI agents need to work within a project's architecture.

It's still evolving, and I certainly don't have every detail figured out.

But I'm increasingly convinced that one of the most useful things we can do for AI-assisted development is make our engineering decisions easier to find, understand, and challenge.

Because ultimately, documentation isn't valuable simply because it exists.

It's valuable when it helps us make better decisions without having to rediscover everything we've already learned.

**Engineering intent is authoritative. Documentation preserves it.**

And once we've preserved that intent, there's another question worth exploring.

How do we make sure the work actually follows it?

That's where the next principle comes in.

**Governance Protects It.**
