---
layout: single
title: "AI Week In Review: Agents Need a Platform, Not Just a Prompt"
date: 2026-11-06 10:00:00 -0600
categories: blog
tags:
  - "AI Week In Review"
  - "Artificial Intelligence"
  - "AI-Assisted Development"
  - "Agentic AI"
series: ai-wir
author: Jeff
comments: true
published: true
---

One thing kept showing up in this week's AI briefings.

We aren't just choosing a model and writing a better prompt anymore. We're building source-control systems for thousands of agents, putting agent instructions inside software packages, keeping long-running conversations in memory, optimizing local inference for repository-scale work, and connecting the whole thing to security tooling.

That looks a lot less like a chatbot and a lot more like a platform.

The model is still important. OpenAI released GPT-6 Cyber this week, and several large open-weight coding models arrived too. But the announcements I keep coming back to are the ones dealing with what happens around the model once an agent starts doing real work.

A prompt can tell an agent what I want. A platform has to preserve the context, enforce the boundaries, coordinate the work, and give me evidence of what actually happened.

## Source control has to survive the agents

OpenAI published a detailed look at [Pando](https://openai.com/index/scaling-git-at-openai/), the Git backend it built for its monorepo.

The reason for building it is interesting. Codex made it possible for engineers to have several changes moving at the same time. That meant more branches, pushes, merges, CI jobs, fetches, and clones. The Git service wasn't just serving more developers. It was serving a different pattern of development.

OpenAI says reliability sometimes fell below 90 percent before the change. Pando now handles concurrent updates to unrelated branches, reuses packfiles across fetches, and keeps the normal Git protocol and client in place. According to OpenAI, it reduced median push latency by nearly five times and now serves almost a petabyte of Git data per day.

Those are OpenAI's numbers, and most of us aren't operating at anything close to that scale. The architectural shift still matters.

When one engineer can start several agents, each agent can create more work for source control, CI, package infrastructure, and review. Giving everyone an agent doesn't just increase coding throughput. It changes the load on everything downstream.

I've seen versions of this with ordinary automation. A script that runs once on a workstation is one thing. The same operation running in parallel across a build farm becomes an infrastructure problem.

Agents don't remove that progression. They get us there faster.

## The instructions are starting to travel with the dependency

Microsoft demonstrated [delivering agent skills inside NuGet packages](https://devblogs.microsoft.com/dotnet/delivering-skills-in-nuget-packages/) this week.

I like the basic idea. A newly released package may contain APIs the model has never seen. Instead of asking the agent to reverse engineer the library from assembly metadata, the package can include a `SKILL.md` file explaining how it should be used.

The skill arrives with the same version of the package it describes. When the API changes, the package author can update the guidance alongside the binaries.

That solves a real problem. Model training will always lag behind current software, and most packages will never appear often enough in training data for a model to know them well anyway.

It also creates a boundary we need to be clear about.

The package should be authoritative about its own API. It should not become authoritative about my application's architecture.

A library can explain how to configure its client, which calls are safe to retry, or how to check for vulnerable dependencies. My repository still needs to decide whether that library belongs in the system, where it can be used, and what validation is required before the change is accepted.

There is a supply-chain question here too. If I wouldn't install a package without considering where it came from, I shouldn't let the instructions inside that package quietly direct an agent without the same review. The skill is not executable code, but it can influence a process that writes and runs code.

Package-owned guidance and project-owned engineering intent can work well together. They are different kinds of authority, and the agent needs to know which one wins when they disagree.

## Conversation state is becoming a runtime concern

Anthropic announced [message threads for the Claude Messages API](https://claude.com/resources/articles/message-threads). Instead of sending the entire conversation with every request, an application sends the full history once and then sends only what changed on later turns.

For active conversations between 500,000 and one million tokens, Anthropic says this reduces median time to first token by about 35 percent. The model still reads the full conversation, so token usage doesn't change. This is a transport and latency improvement, not a discount on the context.

The implementation detail I find useful is that the thread is encrypted in memory, isn't written to disk, and expires five minutes after the last request. After that, the client sends the full conversation again.

That makes message threads useful for an agent actively working through hundreds of tool calls. It does not make them a durable project memory.

I think that distinction is going to matter more as agent platforms add different kinds of memory.

There is session state that keeps the current work moving. There is project knowledge that should survive across sessions. There are architectural decisions that need an authoritative home. There is also temporary context that should disappear when the task ends.

Calling all of that "memory" makes it easy to confuse convenience with durability.

An in-memory thread can make the agent faster. It can't replace the documentation, decisions, and evidence the project needs to keep.

## Local inference is becoming part of the platform

Microsoft also published its work on [running Qwen3.8-27B through Foundry Local](https://devblogs.microsoft.com/foundry/foundry-local-agentic-coding-qwen3-8-27b/) and connecting it to GitHub Copilot.

The model is distributed at four-bit precision at roughly 18 GB. Microsoft says it can run on an NVIDIA GPU with 24 GB of VRAM and support a context window up to 64,000 tokens. Systems with at least 32 GB of VRAM can support longer context up to the model's 262,000-token limit.

There are a lot of vendor benchmark numbers in the announcement, and I would want to test those against my own repositories before drawing conclusions. What interests me is the amount of runtime engineering behind the experience: speculative decoding, memory management, KV-cache quantization, prefix caching, continuous batching, and hardware-specific optimization.

This is another example of the model being only one part of local AI.

Downloading weights doesn't give me a responsive coding agent. I still need an inference runtime that understands the hardware, a tool that can use the model correctly, enough context for the repository, and a way to measure whether the result is actually useful.

Local inference can keep code and context on the machine and give me more control over cost and availability. It doesn't automatically solve access control, secret handling, or validation.

The agent should still receive access to the capabilities it needs without knowing the underlying secrets. Whether the model runs in a cloud service or on a workstation doesn't change that requirement.

## The controls belong outside the agent

The security announcements this week reinforce the same pattern.

OpenAI introduced [GPT-6 Cyber](https://openai.com/index/introducing-gpt-6-cyber/), a specialized model for finding and validating exploitable vulnerabilities. OpenAI reports large improvements over its previous cyber model and says it has used the model to find high-severity Firefox vulnerabilities and develop working Linux kernel exploits.

That capability is useful to defenders, but it also makes the execution boundary more important. OpenAI's own deployment guidance includes sandboxing, action review, monitoring, and human oversight for high-risk work.

Anthropic announced [inference hooks and additional enterprise security controls](https://claude.com/resources/articles/claude-enterprise-security-updates) from the other direction. Prompt hooks can send prompts to an organization's security tooling for review. Pre-tool-call hooks can check an action before it runs. Anthropic is also supporting Endpoint Detection and Response sensors inside the virtual machine used by Claude and Cowork.

I like that architecture because the policy doesn't depend on the agent remembering to apply it.

The organization can keep its existing data-loss-prevention rules and inspect a tool call before the tool executes. The agent gets a decision. It doesn't get to decide whether the security policy applies.

That is the same pattern I use with Key Vault and managed identity. The application or workflow receives the capability it needs without carrying the secret around. In the context of agents, we can extend that beyond credentials to tool permissions, network access, data handling, and approval paths.

The details are different, but the overall idea is the same. The control should live at the boundary it is meant to protect.

## This is starting to look familiar

Once I put these announcements next to each other, the agent platform starts to look a lot like the application platforms we've been building for years.

- Source control and CI have to handle concurrency.
- Dependencies need versioned documentation and supply-chain controls.
- Session state needs a defined lifetime.
- Project knowledge needs an authoritative home.
- Inference needs capacity planning and observability.
- Security policy needs to be enforced outside the workload.

Collectively, I think we need to treat all types of automation, from simple scripts through to complex processes, with the same rigor we would bring to a multi-tiered application.

The conversational interface can make an agent feel informal. The system behind it isn't.

If an agent can modify several repositories, install dependencies, call external tools, use local or hosted models, and continue working across a long session, then it is a distributed workload. It needs the same attention to identity, state, isolation, monitoring, and failure recovery as anything else with that much access.

The prompt starts the work. The platform determines whether the work is safe, repeatable, and worth keeping.

Are you still treating your coding agent like a chat window, or have you started treating it like part of the engineering platform?
