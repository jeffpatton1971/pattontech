---
layout: single
title: "AI Week In Review: The Better Agent Has Better Boundaries"
date: 2026-11-13 10:00:00 -0600
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

Over this past week, I'm noticing a growing trend that I think is worth looking at.

The most interesting improvements aren't all coming from larger models. They're coming from getting more specific about what surrounds the model: which tools it can use, which instructions it should trust, which data those instructions apply to, and what the agent is allowed to execute.

Microsoft put the same model behind a better agent harness and reported better results. Visual Studio made it easier to swap between hosted and local models. Anthropic introduced an experimental mode where Claude writes JavaScript to coordinate its tools. IBM released preview models trained to distinguish instructions from data. OpenAI and llama.cpp both tightened trust boundaries around the environments their agents and servers operate in.

These aren't five unrelated stories. They all point toward the same thing.

The quality of an AI system is increasingly determined by how clearly we define the space around the model.

## The same model can produce a different result

Microsoft's update to [Copilot in Visual Studio](https://devblogs.microsoft.com/visualstudio/copilot-in-visual-studio-just-got-a-major-upgrade/) is probably the cleanest example from this week.

Microsoft moved the Visual Studio experience to a new agent harness built on the Copilot SDK. In its own test across 157 C# coding tasks, the new harness resolved 124 tasks while the previous experience resolved 103. Both used the same model.

Those are Microsoft's results, and I'd want to reproduce them against my own repositories before treating them as universal. The comparison is still useful because it isolates something we tend to overlook. The model didn't change. The system around it did.

The harness decides what context the model sees, what tools are available, what it remembers, and how long it keeps working. Change those things and the same model can behave like a different product.

That matters even more now that [Bring Your Own Model is generally available in Visual Studio](https://devblogs.microsoft.com/visualstudio/bring-your-own-model-is-now-generally-available-in-visual-studio/). Visual Studio can connect to OpenAI, Anthropic, Microsoft Foundry, Ollama, and custom endpoints, including local or internally hosted models. It also lets the user describe capabilities such as context size, tool calling, and vision support.

I like the direction because it makes model choice less permanent. I can compare a local model with a hosted one without rebuilding the rest of the engineering workflow each time.

It also makes the harness easier to evaluate on its own. If several models fail at the same point, I may have a context or tooling problem. If one model succeeds where the others fail, I have a model difference worth investigating. Either way, I learn more than I would by treating the model and the surrounding platform as one black box.

## Context needs more structure than a long prompt

IBM's [LLMON preview](https://huggingface.co/blog/ibm-granite/llmon-an-llm-native-markup-language-now-in-granite) approaches the same problem from another direction.

An agent's context is a mixture of system instructions, conversation history, retrieved documents, tool output, user data, requirements, and past reasoning. Most of that structure is implied. We flatten it into a prompt and hope the model correctly works out which parts are instructions and which parts are only data.

LLMON, short for LLM Object Notation, makes those relationships explicit. It uses model-native markers to identify instructions, data, requirements, and output locations. An execution block then binds an instruction to the specific data it is supposed to operate on.

That binding is the part I find interesting.

If I ask an agent to summarize a document, the words inside that document shouldn't suddenly gain the same authority as the instruction that asked for the summary. IBM trained two experimental Granite 4.1 preview models around that distinction. The company reports substantially better resistance to static indirect prompt injection in its testing, while also saying that adaptive attacks across multiple turns still need more work.

I wouldn't treat LLMON as an authorization system, and IBM doesn't present these preview models as a complete answer. A model recognizing that something is data is helpful. Deterministic controls still need to decide whether a tool call is allowed.

What it does show is that context can't remain an undifferentiated pile of text as agents take on more work. Instructions need identity. Data needs identity. The relationship between them needs to be something we can describe and test.

## Generated orchestration is still software

Anthropic's [Claude Code 2.1.323 release](https://github.com/anthropics/claude-code/releases/tag/v2.1.323) adds an experimental Code Mode where Claude writes JavaScript to call its tools.

There is a practical advantage to that approach. A loop, a branch, or a sequence of tool calls can be expressed as code instead of requiring the model to repeatedly choose one operation, wait for the result, and decide what to do next. For a repeatable workflow, that could reduce some of the conversational overhead and make the sequence easier to understand.

But once an agent generates code that coordinates tools, I want to treat that code the same way I would treat any other automation.

What is it allowed to call? Which arguments are acceptable? Can I inspect the plan before it runs? What happens when a tool fails halfway through? Is the result attributable to the agent and the policy that allowed it?

This is also where secret handling becomes part of the agent design. An agent may need the capability to deploy an application, query a service, or publish an artifact. That doesn't mean it needs to know the underlying credential. It should receive access to the capability it needs without the secret becoming part of its prompt, generated script, log, or conversation history.

The details are different from a traditional application, but the overall idea is the same. In the context of agents, we can extend patterns such as Key Vault beyond the application or workflow. The agent gains access to what it needs without knowing the secret itself.

Collectively, I think we need to treat every kind of automation, from a simple script through a complex agent process, with the same rigor we would bring to a multi-tiered application. Generated JavaScript doesn't get a pass because a model wrote it.

## Trust has to be explicit

Two Friday releases reinforced that point.

[Codex 0.169.0](https://github.com/openai/codex/releases/tag/rust-v0.169.0) fixed a case where running `codex exec` with workspace write access could have enabled project configuration or MCP servers in an untrusted directory. The updated behavior keeps those capabilities disabled until the user explicitly trusts the project.

That is a small release-note item with a much larger idea behind it. A repository contains data, but it can also contain instructions and configuration. Opening a repository shouldn't automatically grant everything inside it authority to extend the agent.

[llama.cpp 1.0.0](https://github.com/ggml-org/llama.cpp/releases/tag/v1.0.0) landed the same day with server hardening for SSRF, CORS, DNS rebinding, and MCP tool cancellation. I tend to think of local inference as a way to keep more control over the stack, but local does not automatically mean safe. The moment a model server retrieves remote content, accepts network traffic, or invokes tools, it has the same kinds of trust boundaries we already know how to mishandle in other systems.

The useful shift here is from implicit trust to explicit trust. A directory isn't trusted because I opened it. A document isn't authoritative because it is in the context window. A local server isn't secure because it runs on hardware I own. A generated program isn't safe because it came from an assistant.

Every one of those relationships needs a boundary.

## This is the layer llamarc42 is meant to preserve

This week's announcements also sound a lot like the direction I'm taking with [llamarc42](https://github.com/llamarc42).

Llamarc42 isn't intended to replace source control, inference runtimes, or agent orchestration. It provides a project-owned place to preserve engineering intent, architectural decisions, requirements, and governance independently of whichever agent or model is doing the work.

That separation becomes more valuable when the model is swappable and the orchestration layer keeps changing. The model can do the work. The agent platform can provide the tools and capabilities. The project still needs to establish what both are supposed to do, what they aren't allowed to do, and how we decide whether the result is acceptable.

LLMON is exploring how a model can distinguish instructions from data. Agent harnesses are getting better at presenting context and tools. Code Mode is exploring more explicit orchestration. Llamarc42 sits at a different layer: it preserves the project's intent so those systems have something stable to work from.

That doesn't eliminate model error, and it doesn't replace permission enforcement or tests. It gives those controls a project-owned source of truth.

## The boundary is becoming part of the product

For a long time, AI product comparisons mostly came down to which model produced the best answer. That is becoming less useful on its own.

I also want to know what context the model received, how instructions were separated from data, which tools it could reach, how trust was established, where credentials were resolved, and what evidence the system kept after the work finished.

Those are engineering questions, not prompt-writing questions.

The model still matters. But this week made it harder to ignore how much value now lives in the harness, the runtime, the policy, and the project context surrounding it.

The better agent may not be the one with the smartest model. It may be the one with the clearest boundaries.
