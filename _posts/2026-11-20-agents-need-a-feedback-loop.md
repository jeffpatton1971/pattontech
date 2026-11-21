---
layout: single
title: "AI Week In Review: Agents Need a Feedback Loop"
date: 2026-11-20 10:00:00 -0600
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

This week, I kept coming back to one question: how do we know an agent is actually getting better?

There were plenty of new models to compare. OpenAI released GPT-6.2 Astra and Sol, Microsoft introduced two MAI Code models, and Google released Gemini 4 Neon. Each vendor has its own performance and cost story, but model announcements weren't the part of the week I found most interesting.

The more important changes were around the feedback loop.

GitHub introduced repeatable evaluations for agents, skills, and MCP tools. Microsoft described using production traces to improve agents over time. GitHub added event streams so an agent can wait for a pull request to change instead of polling for it. OpenAI expanded Codex around remote sessions and multi-agent workspaces. Azure AI Gateway reached general availability as a central policy and observability layer across models and tools.

Together, these changes make agent development look a lot more like the rest of software engineering. We define the behavior we want, run the system, capture evidence, compare the result to the expectation, and decide what changes are safe to ship.

The model is part of that loop. It isn't the loop by itself.

## One good run isn't evidence

[GitHub Copilot Evals](https://github.blog/changelog/2026-11-17-github-copilot-evals-is-now-in-public-preview/) entered public preview this week. GitHub describes an eval as a repeatable test for AI behavior: give an agent a realistic task, describe what a good result looks like, and check both the result and how the agent got there.

I like that last part.

With normal software, it usually isn't enough for a test to end with the right value if the system got there by writing to the wrong database or bypassing an authorization check. Agent work should be treated the same way. The final answer matters, but so do the tools it called, the files it changed, the permissions it used, and the evidence it produced along the way.

The need for repeated runs matters too. A single successful agent session can be encouraging, but it doesn't tell me whether the skill is reliable. It definitely doesn't tell me whether changing the model, prompt, MCP server, or tool description created a regression somewhere else.

GitHub's preview lets a developer run the same eval before and after a customization change and inspect which checks passed or failed. That is still an early implementation, but the underlying practice is the important part.

I want agent behavior captured as something the project can rerun. The test should live with the project, describe an outcome the project cares about, and survive whichever model happens to be selected next month.

## Production traces can close the loop

Pre-release evals tell me whether an agent works against the cases I already know about. Production tells me what I forgot to test.

Microsoft's announcement about [continuous improvement in Foundry](https://azure.microsoft.com/en-us/blog/continuous-improvement-comes-to-microsoft-foundry/) connects those two sides. Microsoft says Foundry can feed agent traces and user input back into an improvement process, test possible changes, compare tradeoffs across quality, speed, and cost, and leave the final shipping decision with the team.

That sounds useful, but I would be careful with the phrase "continuous improvement." I don't want an agent changing itself in production because it saw one unusual trace. I want a controlled process where traces identify a gap, that gap becomes a test, a proposed change is evaluated against the full suite, and a person decides whether the evidence is strong enough to ship it.

In other words, the loop should look familiar:

1. Observe what happened.
2. Turn the failure or edge case into a repeatable evaluation.
3. Change the prompt, skill, tool, policy, or model.
4. Run the existing evaluations again.
5. Review the quality, cost, latency, and security tradeoffs.
6. Promote the change through the same release process as anything else.

That is a much stronger definition of learning than allowing the runtime to quietly drift.

It also gives model comparisons more meaning. The best model isn't automatically the one with the highest general benchmark score. It is the one that passes the project's evaluations at an acceptable cost and latency without crossing the project's boundaries.

## Long-running agents need events, not busy loops

The feedback loop also changes how agents wait for the rest of the engineering system.

GitHub CLI now has preview support for [waiting on pull request events](https://github.blog/changelog/2026-11-18-wait-for-pull-request-events-with-github-cli/). An agent can subscribe to things such as a submitted review, a new commit, a merge conflict, or a completed check suite. The command returns structured event data and can exit after one event, wait for several, or keep listening.

That is a small feature with a useful architectural implication.

An agent working on a pull request doesn't need to ask GitHub every few seconds whether anything changed. It can stop, wait for the event it cares about, and resume when there is actually new information. That reduces unnecessary traffic, but more importantly, it gives the workflow a clearer state transition.

[Codex 0.171.0](https://github.com/openai/codex/releases/tag/rust-v0.171.0) moved in a related direction by adding remote noninteractive sessions and making its workspace view, including subagent transcripts, the default. Remote and multi-agent work make it easier to let a task continue beyond one terminal session. They also make it more important to know which event woke the agent up, which context it received, and which permissions still applied when it resumed.

I've seen long-running automation become difficult to reason about when waiting, retrying, and state are hidden inside one large loop. Agents won't improve that on their own. If anything, an agent that can improvise makes those transitions more important to record.

Whether the automation is a ten-line script or a multi-agent workflow, I want the same basic things: explicit state, bounded retries, durable events, attributable actions, and a way to resume without pretending nothing happened while it was away.

## A gateway should provide capability without handing over the secret

[Azure AI Gateway](https://techcommunity.microsoft.com/blog/integrationsonazureblog/whats-new-in-azure-api-management-at-microsoft-ignite-2026/4574194) also reached general availability this week. Microsoft positions it as a central layer for routing, identity, access policies, cost controls, and observability across models and MCP tools from multiple providers.

This is Azure supporting the AI story rather than being the story itself.

As agents move between models and tools, I don't want every agent implementation to carry provider-specific URLs, credentials, rate limits, and failover logic. I want it to receive a stable capability that the platform can govern.

The same principle applies to MCP tools. An agent may be allowed to query a deployment system or publish an artifact. That does not mean the provider key or service credential should become part of the prompt, generated code, log, or conversation history. The gateway or execution environment should resolve the secret and enforce the policy at the point of use.

The agent gets access to the capability it needs without knowing the underlying secret.

Centralization isn't automatically good, of course. A gateway can become a large blast radius if its policies are vague or its own credentials are too broad. It still needs least privilege, useful audit records, clear ownership, and tests that prove the rules work as intended.

That is the same rigor I would bring to a multi-tiered application, and I think it belongs across all automation—from simple scripts through long-running agent processes.

## Llamarc42 can connect intent to evidence

This week's feedback-loop theme fits directly with the direction I'm taking with [llamarc42](https://github.com/llamarc42).

Llamarc42 is meant to preserve project-owned engineering intent: the requirements, architectural decisions, constraints, and acceptance criteria that shouldn't disappear when a conversation ends or a model changes.

That intent can provide the stable side of an evaluation.

The agent produces a result and a trace. The project defines what acceptable means. An evaluation compares the two. If a production trace exposes a gap, the project can capture the new requirement and turn it into evidence that every future agent or model has to satisfy.

This is why I don't see llamarc42 as a replacement for an agent platform, an eval runner, or a gateway. Those systems execute, measure, and enforce. Llamarc42 gives them project-owned context and expectations to work from.

Without that layer, it is easy for an evaluation to become a collection of whatever examples happened to fail recently. With it, the tests can stay connected to the reason the system exists and the boundaries it is expected to respect.

## Better agents need better engineering loops

The new models from this week will get most of the attention, and some of them may turn out to be meaningfully better. I still want to test that improvement against work I actually care about.

Can the agent complete the task more reliably? Did it use the right tools? Did it stay inside its permissions? Can I explain why it resumed? Did the change improve quality without making cost or latency unacceptable? Can I rerun the same evaluation after switching models?

Those questions are becoming part of the product, not something we add after the demo works.

An agent that can act needs boundaries. An agent that runs for a long time needs observable state. An agent that changes needs repeatable evaluations. And an agent that improves needs a feedback loop the team can inspect and control.

That is the part of this week's AI news I expect to matter long after the model leaderboard changes again.
