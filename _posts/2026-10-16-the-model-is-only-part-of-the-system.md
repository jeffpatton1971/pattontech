---
layout: single
title:  "AI Week In Review: The Model Is Only Part of the System"
date: 2026-10-16 10:00:00 -0500
categories: blog
tags:
    - October
    - "2026"
    - AI Week In Review
    - ArtificialIntelligence
    - AIAssistedDevelopment
author: Jeff
comments: true
published: true
---

I've been getting a daily AI briefing that covers OpenAI, Anthropic, Microsoft, Ollama, open models, and whatever else happened overnight that I should probably know about.

It works pretty well, but by the end of the week I have five days of announcements, benchmarks, new models, and new products to sort through. That's a lot of "important" news, and not all of it is going to matter a week from now.

Over this past week, I'm noticing a growing trend that I think is worth looking at.

The models are still getting better, obviously, but more of the interesting work is happening around them. How do we decide which model should do the work? Where do we let an agent run? What can it access? How do we give it access to a secret without actually handing over the secret?

The model is important, but it is only part of the system.

## Does this really need the biggest model?

Both OpenAI and Microsoft made announcements around specialized decision models this week.

OpenAI released its [Decisions API](https://developers.openai.com/api/docs/guides/decisions) in public beta. It takes text or images and returns something an application can actually use, like a probability, a choice from a fixed list, or a score. Microsoft also released [Microsoft-Decision-1](https://commandline.microsoft.com/microsoft-decision-1-model-foundry/), which is based on Qwen3.5-9B and is intended for things like routing, classification, prioritization, and workflow control.

Neither one is trying to have a conversation with you, and that's kind of the point.

An AI agent runs into questions like these all the time:

- Did this step work?
- Should I retry, keep going, or stop?
- Which model should handle the next task?
- Does a person need to review this?

We could send each question to the smartest model we have, ask it to reason through the problem, generate a nice paragraph, and then write code to turn that paragraph back into a yes or no. That feels a little silly when the actual question is, "Should I keep going?"

I like the idea of using regular code when the rules are known, a smaller decision model when there are a few possible answers, and a larger model when the problem is actually hard.

Microsoft says Decision-1 is roughly 35 times faster than GPT-6 Sol in its testing. OpenAI says the Decisions API can return answers about 10 times faster than its Responses API. Those are their numbers, so I would still want to test this against my own workload before making any real decisions based on them.

The exact numbers aren't really what caught my attention anyway. What I like is the idea that every step in an agent workflow doesn't need the same kind of intelligence, and it definitely doesn't need to cost the same amount.

## Open weight doesn't always mean local

Meta released [Muse Spark Open](https://research.meta.ai/blog/muse-spark-open) on Thursday, and on paper it is pretty wild. It has 816 billion total parameters, 42 billion active parameters, a one-million-token context window, and support for text, images, audio, and video as input.

The weights are available to download, and the model supports inference engines including llama.cpp, MLX, SGLang, and vLLM. Meta also demonstrated it running locally across two Mac Studios with M5 Ultra processors and 512GB of unified memory in each one.

I love that we've reached a point where a model this large can run outside of a datacenter. I also have to laugh a little at calling a pair of high-end Mac Studios with a terabyte of combined memory a desktop setup.

According to the [model card](https://huggingface.co/meta-models/Muse-Spark-Open-816B-A42B-NVFP4), the NVFP4 version needs at least 700GB of GPU memory, while BF16 needs around 2TB. It also uses Meta's Muse Model License rather than something like Apache 2.0, so the license still needs to be part of the conversation.

Open weights give us more control over where a model runs and who operates it, but that doesn't automatically make the model practical to run at home. For most of us, smaller coding models are still going to make more sense for local work.

What interests me is the direction this is going. The model provider is becoming one choice inside a much larger system, but the hardware, inference engine, license, and operating environment still determine what we can actually do with it.

## Give the agent somewhere safe to work

Ollama and Fly.io announced an integration for running coding agents inside [Fly.io Sprites](https://ollama.com/blog/fly-io-sprites). A Sprite is a persistent Linux virtual machine that acts as a sandbox, and it supports tools including Claude Code, Codex, and Gemini CLI.

The part that caught my attention is how they handle the Ollama API key. The key is stored by Fly.io and added to requests through a gateway. The agent gets a gateway address and a connection ID, but it never gets the actual key.

If the agent doesn't have the secret, it can't print it to a log, commit it to a repository, or helpfully include it in an error message.

That reminds me a lot of how I use Azure Key Vault. An application running in App Service can access what it needs from Key Vault, and I can do the same thing from an Azure DevOps pipeline. I can even reference Key Vault from a GitHub workflow; I just need to tell the workflow how it is allowed to access it.

The details are different, but the overall idea is the same. In the context of Fly.io, we can extend this beyond the application or workflow and now include agents. They gain access to the secrets they need without knowing the secrets.

Collectively, I think we need to treat all types of automation, from simple scripts through to complex processes, with the same rigor we would bring to a multi-tiered application.

The sandbox matters for an even simpler reason: coding agents run code. Sometimes it is code they wrote, sometimes it came from a repository, and sometimes it came from a dependency nobody has looked at yet. The agent could be doing exactly what we asked and still run something we really didn't want running on our machine.

That isn't a new problem created by AI. It is why we isolate builds, use short-lived credentials, limit network access, and avoid running unknown code on the same machine that has access to everything else.

AI just makes it easier to forget those rules because asking an agent to do something feels more like talking to a coworker than starting an automated process.

## This is the part I'm watching

Muse Spark Open was probably the biggest model announcement for me this week, but these are the pieces I keep coming back to:

- A smaller model that can make a limited decision without writing an essay first.
- An open-weight model where the hardware and license matter just as much as getting the weights.
- A sandbox where an agent can make a mistake without taking the rest of the system with it.
- A credential the agent can use without ever being able to see it.

None of that is as exciting as announcing a new frontier model, but this is the stuff that makes AI useful in an actual engineering environment.

I've spent a lot of time lately thinking about engineering intent, documentation, and governance for AI-assisted development. The better these tools get, the less I want to rely on the model itself to provide the guardrails.

I want the model to help do the work. I want the system around it to decide what work it can do, what it can access, and when a person needs to get involved.

I'm still figuring out what that looks like in my own projects, especially when it comes to local models and coding agents. I don't think the answer is finding one model smart enough to trust with everything. I think it is building an environment where several models can be useful without giving any of them more access than they need.

Are you putting real boundaries around your coding agents yet, or are they still running on your workstation with access to everything?
