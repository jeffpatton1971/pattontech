---
layout: single
title:  "AI Week In Review: The Agent Shouldn't Control the Guardrails"
date: 2026-10-23 10:00:00 -0500
categories: blog
tags:
  - "AI Week In Review"
  - "Artificial Intelligence"
  - "AI-Assisted Development"
  - "Governance"
author: Jeff
comments: true
published: true
---

I've been reading through this week's AI briefings, and one story keeps pulling the rest of them into focus.

The models can do more. The tools around them can do more. We can run specialized models locally, hand work to coding agents, and let those agents operate across a repository with less supervision than we would have considered reasonable a year ago.

At the same time, we're still learning where the boundaries need to be.

Over this past week, I'm noticing a growing trend that I think is worth looking at. If an agent can change the system, it shouldn't also control the monitoring that tells us whether the change was safe.

That sounds like a narrow security rule, but I think it applies to almost every part of how we're building agentic systems.

## The agent turned off the alarm

OpenAI published an [incident report](https://alignment.openai.com/misalignment-reports/suppressing-misalignment-monitoring-alerts-and-disabling-misalignment-monitoring/) this week covering two internal coding tasks where agents weakened monitoring while doing otherwise legitimate engineering work.

In the first case, a GPT-6.1 Sol agent was working on an evaluation pipeline. The user had previously told it not to send messages while searching Slack. Much later in the task, the agent applied that instruction to the monitoring system. It emptied the lists of people who would receive alerts and set `no_notify` to true.

The monitor still ran and stored its findings, but the people who were supposed to see those findings would not be notified.

In the second case, a GPT-6 Astra agent was asked to make one implementation match another. The reference implementation didn't include the existing monitoring integration, so the agent removed it. It also replaced a test that checked for monitoring with one that checked that monitoring was not present. When the user later asked whether it had removed the monitor, the agent said it had not.

OpenAI has not concluded that either model was deliberately trying to evade oversight. Their report says this may be overpersistence: the agent pushed too hard to finish the task and treated ordinary instructions as permission to weaken a safeguard.

I'm glad they're being careful about that distinction. Intent matters when we're trying to understand model behavior, but it doesn't change the engineering problem.

The alarm was under the control of the thing being monitored.

If the same process can do the work, change the monitoring, decide whether an alert should be sent, and then tell me whether everything went well, I don't really have independent oversight. I have a process reviewing itself.

OpenAI's response points in the right direction. They say monitoring for Codex workloads runs in the service handling model requests, outside the agent's direct control. They've also updated an independent review step to block attempts to disable monitoring and are working toward terminating workloads that don't meet their monitoring requirements.

That's the architecture I want. The agent can propose and implement a change. Something outside the agent's authority verifies what happened. The evidence is preserved, and a separate policy decides whether the result is accepted.

## A permission rule has to stay a permission rule

Anthropic's [Claude Code 2.1.309 release](https://github.com/anthropics/claude-code/releases/tag/v2.1.309) arrived this week with a long list of security, sandbox, and permission fixes.

A sandboxed Bash command could run shell startup files outside the sandbox. A write-deny pattern on macOS could be missed when a path was reached through a symbolic link. An edit deny rule did not always block a Bash redirect to the same file. Some compound commands didn't trigger the permission prompt they should have, and a disabled MCP server could be reconnected by a later prompt.

Those are different bugs, but they all land in the same place: the boundary has to hold no matter which path the agent takes to reach it.

It isn't enough to say the agent can't edit a file if it can redirect command output into that file. It isn't enough to sandbox the command if the shell can load startup code before the sandbox is in place. It isn't enough to disable a server if another part of the permission flow can quietly turn it back on.

I don't see that list as a reason to avoid coding agents. I see it as evidence that these systems are becoming real development infrastructure. Real infrastructure has edge cases, and security boundaries have to be enforced at the layer where those edge cases can actually be contained.

We've dealt with versions of this problem for years. An application can be told not to read a database table, but I would still enforce that in the database permissions. A deployment pipeline can be told not to touch production, but I would still separate the environments and credentials. The instruction is useful, but the control belongs outside the process receiving the instruction.

Agents don't change that principle. They make it more important.

## Local AI is becoming more than a model runner

Ollama also released [version 0.40.9](https://github.com/ollama/ollama/releases/tag/v0.40.9) this week, and it is a good example of how quickly the runtime around local models is expanding.

Ollama can now stream speech using Gradium's Phonon models through an OpenAI-compatible audio endpoint. It added the d3 family of 2B, 4B, 9B, and 27B decision models, which can classify input, answer yes-or-no questions, or score a set of options. Recursive's Flint coding model now runs on Linux and Windows in addition to macOS.

There were also less exciting fixes that probably matter more in production. Streaming requests now report generation failures instead of quietly ending after partial output, and Ollama passes model context lengths and capabilities to coding tools so they don't make the wrong assumptions about compaction or available features.

Taken together, Ollama is starting to look less like a command I use to run a local chat model and more like a common runtime for several kinds of AI work.

I can use one model to write or reason, another to make a limited decision, and another to generate speech. A coding agent can use those services without every step going through the same large model or the same provider.

I like that direction, but it also means I need to think about the runtime as part of the application architecture. Which model is allowed to handle which data? What happens when streaming fails halfway through a response? Does the calling tool understand the model's actual context limit? Which license applies to the model I selected? Where is the decision recorded if a small model is controlling the next step in a workflow?

Running the model locally answers the question of where inference happens. It doesn't answer the rest of those questions for me.

## The guardrails are part of the system

Last week I wrote about giving an agent access to the secrets it needs without giving it the secrets themselves. This week's monitoring story is the same architectural idea applied to oversight.

The agent should be able to use a capability without owning the control that governs that capability.

That might mean a credential broker that signs a request without exposing the API key. It might mean a sandbox that enforces which paths can be changed. It might mean monitoring that the agent cannot disable, or an approval step performed by a separate service using evidence the agent cannot rewrite.

The details will be different from one system to another, but the overall idea is the same. We need to treat all types of automation, from simple scripts through to complex processes, with the same rigor we would bring to a multi-tiered application.

For an agentic workflow, I think that means separating at least four things:

- The model deciding what to do.
- The runtime deciding what it is allowed to do.
- The monitoring recording what it actually did.
- The policy deciding whether the result can move forward.

Those can be simple pieces. Not every personal project needs an enterprise security platform. What matters is that the model doesn't get to reinterpret all four roles whenever they become inconvenient.

I want agents to be persistent. I want them to work through a difficult problem, recover from failures, and keep going without asking me about every small decision. That's a large part of what makes them useful.

I just don't want persistence to include turning off the alarm so the job can finish.

The model can do the work. The system around it still needs to decide what it can access, what evidence must be kept, and who gets to say the work is done.

If your coding agent can change its own guardrails, are they really guardrails?
