---
layout: single
title: "Understand the Problem Before Solving It"
date: 2026-11-03
series: engineering-with-ai
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

## Engineering with AI · Prompt 01 · Discover & Challenge

One thing I've noticed when working with AI on software engineering problems is how quickly a conversation can move from describing an issue to proposing a solution.

I can describe something that isn't working, and within seconds, AI may suggest configuration changes, new components, or an implementation plan. Sometimes those suggestions are useful. Other times, I find myself questioning whether we've established what the problem actually is.

The difficulty is that a proposed solution can sound reasonable even when it's based on assumptions we haven't examined.

I've run into situations where the initial description of a problem turned out to be incomplete. What appeared to be a configuration issue might involve an architectural decision, an unexpected dependency, or behavior that was never clearly defined in the first place.

If we start implementing immediately, we risk building a solution around our initial understanding rather than the actual problem.

This is where I think a more deliberate conversation with AI can help.

## The Problem with Starting at the Solution

Consider a hypothetical situation where a team is experiencing unreliable deployments.

Someone might ask AI:

> Our Azure DevOps pipeline keeps failing intermittently. Help me redesign it to make deployments more reliable.

That's a reasonable request, but it already assumes that redesigning the pipeline is the appropriate solution.

AI might suggest separating build and deployment stages, adding retries, introducing deployment gates, or changing how artifacts are managed.

Any of those changes could be useful. We just don't know yet.

What does "intermittently" mean? Are failures occurring during builds, deployments, or application startup? Are they happening across every environment or only production? Did something change recently? Are the failures caused by the pipeline, or is the pipeline reporting failures in something else?

Until we've answered at least some of those questions, we don't have much evidence that a redesign would address the problem.

I'd rather use AI to help investigate those questions before deciding what to change.

## Prompt 01: Understand the Problem Before Solving It

The purpose of this prompt is to keep AI in discovery mode long enough to establish a useful understanding of the problem.

It asks the model to distinguish observations from assumptions, challenge the proposed solution, and identify missing information that could change the direction of the work.

It also establishes a boundary: don't begin designing or implementing until we've agreed on what we're trying to solve.

### The Prompt

```text
I want to explore an engineering problem before deciding how to solve it.

The problem or idea:
[Describe the problem in your own words. Include what you've observed,
what you expected, and any solution you're already considering.]

Your role is to help me understand the problem, not immediately solve it.

Start by reviewing my description.

1. Restate what you believe the actual problem is. Distinguish the
   observed behavior from my interpretation of its cause and any
   solution I've proposed.

2. Identify what is known, what is assumed, and what information is
   missing. Don't present an assumption as an established fact.

3. Challenge assumptions that could materially change our understanding
   of the problem. Explain why each matters.

4. Ask focused questions, one at a time, when the answers would change
   the direction of the investigation. Don't ask questions that my
   description has already answered.

5. Identify relevant constraints, dependencies, existing decisions,
   and system boundaries. Don't invent requirements or architecture.

6. If additional evidence would help, explain what to inspect or
   measure and what each result would tell us.

Once the problem is sufficiently understood, summarize:

- The problem we're actually trying to solve.
- What we've established through evidence.
- What remains uncertain.
- The constraints and boundaries that matter.
- Possible approaches worth evaluating, without selecting one yet.

Don't design or implement a solution during discovery.

If you believe I've misunderstood the problem, explain your reasoning
and let me decide whether to revise the problem statement.

Finish by recommending the next decision or investigation we should
undertake. Wait for my approval before moving into solution design.
```

## How I Would Use It

Let's return to the unreliable deployment pipeline.

Instead of asking AI to redesign the pipeline, I'd start with something like this:

> We're experiencing intermittent deployment failures in our Azure DevOps pipelines. The failures appear to happen more frequently when deploying to production, although I haven't confirmed that. We've been discussing whether to redesign the pipeline, but I want to understand what's happening before committing to that approach.

The prompt should encourage AI to separate the information we've provided into what we know and what we're assuming.

For example:

| Observation or statement | Classification |
| --- | --- |
| Some deployments fail | Reported observation, not yet independently verified |
| Failures appear more common in production | Hypothesis requiring evidence |
| The pipeline design is responsible | Unverified assumption |
| Redesigning the pipeline will resolve the issue | Proposed solution without established justification |

This is a useful starting point because it makes the uncertainty visible.

From there, AI might ask whether failures occur during the same pipeline stage, whether deployment logs identify a consistent error, or whether the failures correlate with particular environments or application versions.

Each answer can help narrow the investigation.

If the evidence eventually points to application startup failures, redesigning the pipeline may accomplish very little. If it points to inconsistent artifact handling, the solution might be much smaller than originally anticipated.

Those are hypothetical outcomes, not results from a real investigation. The point is that we should allow the evidence to influence the solution rather than assuming our first idea is correct.

## What I'm Trying to Accomplish

There are a few deliberate choices in this prompt.

The first is separating observations from interpretations. When someone describes a problem, they often include an explanation of why they think it's happening. That explanation may be correct, but it shouldn't automatically become an established requirement or fact.

The second is asking questions selectively. I don't want AI to respond with twenty generic discovery questions every time I describe an issue. I want it to identify the missing information that could materially change our understanding.

The third is preserving the boundary between discovery and design.

This is something I've found particularly useful when working with AI agents. Once an agent starts producing an implementation plan, the conversation can become focused on improving that plan rather than questioning whether the underlying approach is appropriate.

By explicitly stopping at the end of discovery, I have an opportunity to review what we've established before deciding how to proceed.

## Where This Prompt Might Fall Short

I don't think this prompt should be used for every engineering question.

If I know exactly what's wrong and need help remembering a command or understanding a documented API behavior, forcing a discovery process would probably make the interaction less useful.

The prompt also depends on the information available to the model. If I'm missing critical evidence, AI may help identify what to investigate, but it can't establish facts that aren't present.

There's another potential problem: the model might become too cautious, repeatedly asking questions even after we've gathered enough information to make a reasonable decision.

That's why the prompt asks for questions that materially affect the investigation. The goal is to improve understanding without turning every engineering task into an unnecessarily long discussion.

I also wouldn't treat the model's classification of facts and assumptions as authoritative. It can misinterpret evidence or overlook information. Those classifications are useful for organizing the investigation, but they still need to be reviewed.

## What Comes Next

This first prompt is concerned with understanding the problem before deciding how to solve it.

The next prompt will explore something closely related: **Challenge My Assumptions**.

Even when we believe we understand the problem, we can become attached to a particular interpretation or approach. AI can reinforce that attachment by agreeing with our reasoning and producing a convincing explanation for why we're right.

I'd like to explore how we can encourage AI to provide a more useful kind of criticism, one that challenges our reasoning without manufacturing objections simply for the sake of disagreement.

That's where we'll go next.
