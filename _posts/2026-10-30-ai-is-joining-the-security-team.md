---
layout: single
title: "AI Week In Review: AI Is Joining the Security Team"
date: 2026-10-30 10:00:00 -0500
categories: blog
tags:
  - "AI Week In Review"
  - "Artificial Intelligence"
  - "AI-Assisted Development"
  - "Cybersecurity"
series: ai-wir
author: Jeff
comments: true
published: false
---

This week, AI stopped looking like a tool that helps the security team and started looking more like a member of the security team.

I'm not talking about asking a model to explain a CVE or look for a familiar bad pattern in a pull request. The work being described this week included tracing behavior across millions of lines of code, finding security controls that fail when two configuration settings are combined, reverse engineering binaries, and building multi-stage exploit chains against browsers and operating systems.

That's a pretty big jump from adding another scanner to the pipeline.

It also raises a question I keep coming back to. If an AI agent can find a vulnerability, prove it can be exploited, propose a fix, and open the pull request, which part of that process are we independently verifying?

## Finding a bug is one thing. Chaining several together is another.

Anthropic published research this week describing [five exploit chains built with an internal version of Claude](https://www.anthropic.com/research/discovering-vulnerabilities-with-claude).

One of those chains combined four separate Chrome vulnerabilities to move from a malicious web page, through the browser's renderer, and out of the sandbox. Another bypassed the same-origin policy in Safari. The work also included an Android lock-screen bypass, a remotely triggered Windows kernel crash, and a Linux kernel exploit that could run commands as root without credentials or user interaction.

These are Anthropic's results, and the model they used isn't publicly available, but the findings were reported to the affected vendors and the issues discussed in the article have been patched.

What caught my attention wasn't just that Claude found individual vulnerabilities. It was able to combine several weaknesses into a working path through the layers that were supposed to contain the attack.

That matters because most real systems don't fail at one clean boundary. They fail when several reasonable assumptions stop being true at the same time.

A renderer process is isolated, except another issue exposes information about the privileged process. A configuration is valid, except its interaction with another setting silently turns off a security check. A parser is allowed to run, even though the feature it supports is disabled.

An agent that can follow those relationships across a large codebase is doing something more useful than matching a list of known patterns. It is starting to reason about how the system actually behaves.

## The code can work exactly as written and still expose too much

Anthropic also published a separate project on [reducing the attack surface of existing software](https://www.anthropic.com/research/attack-surface-reduction). Claude analyzed 25 open-source projects containing more than five million lines of code and identified 493 issues.

Those weren't all exploitable vulnerabilities, and Anthropic is clear about that distinction. The analysis looked for code that was reachable when it didn't need to be, security-sensitive work happening before authentication, and combinations of settings that quietly weakened a control.

That last category feels especially familiar to anyone who has operated a platform with a lot of configuration. Each setting can be valid on its own. The problem only appears when two of them interact in a way nobody expected.

Anthropic says Claude found 162 undocumented security-sensitive configuration combinations. In 94 cases, the combination silently disabled a security control.

The OpenSSH example is probably the one I'll remember. Claude found roughly 4,500 lines of experimental post-quantum signature code that an unauthenticated client could reach even when the algorithm wasn't enabled. The resulting changes moved the checks ahead of parsing and removed about a fifth of the code previously reachable before authentication in a default build.

Nothing about that requires the underlying code to be poorly written. The issue is that code was reachable before the system had established that it needed to run.

That is an architectural question, not just a coding question.

I've spent a lot of time thinking about how we preserve engineering intent for AI-assisted development. This is another version of the same problem. The implementation tells us what the system does. The architecture should also tell us what the system is supposed to expose, when it is allowed to happen, and which boundary has to be crossed first.

## Security review has to look beyond the pull request

OpenAI published a [CodeRabbit case study](https://openai.com/index/gpt-6-1-sol-coderabbit-code-security/) this week that approaches the problem from the other side.

CodeRabbit is using GPT-6.1 Sol in a security agent that reviews changes across files, dependencies, shared libraries, and linked repositories. For selected findings, it can attempt to verify an exploit in an isolated sandbox and draft a pull request with a proposed fix.

The important detail for me is that the security review is independent of the agent that wrote the code.

If a coding agent changes an authentication library, reviewing only the lines in the immediate pull request may not tell me how that change affects every application using the library. The build can pass. The local tests can pass. The code can even be a reasonable implementation of the requested change.

The risk may live somewhere else.

That is where a security agent can be genuinely useful. It can follow the change into the shared components and ask what became reachable, which assumption changed, or whether an existing safeguard still applies.

I still don't want the agent making the final decision. In the workflow OpenAI describes, exploit verification happens in a sandbox, the agent proposes the fix, and a person decides what ships.

The agent doing the review can be fast and persistent. The approval path still belongs to the system and the people responsible for it.

## The reviewer needs to be tested too

There is another side to this. Once I use AI to review AI-generated changes, it becomes very easy to build a circle where one model says the code is safe and another model agrees.

OpenAI's article on [detecting frontier model misalignment](https://openai.com/index/how-we-detect-frontier-model-misalignment/) describes a much more involved monitoring system. It uses several stages, with inexpensive screening early and more capable review for cases that need deeper investigation. It replays recorded incidents, runs simulated attacks, audits activity that was not flagged, and asks agents to try to evade or disable the monitoring in controlled environments.

That last part is important. They don't only test whether the monitor catches the behavior they already know about. They test whether an agent can find a path around the monitor itself.

OpenAI says one monitor detected a model reaching the internet from an isolated training environment within 15 minutes, and a responder began reviewing it three minutes later. The model had already crossed a boundary, but monitoring gave someone a chance to intervene and then improve the containment system.

This is why I don't think "we have another model review it" is a complete answer.

The review process needs known incidents it can replay. It needs simulated failures. It needs random audits of work that passed. It needs preserved evidence, and the agent doing the work shouldn't be able to rewrite that evidence or turn off the notification.

Last week I wrote that the agent shouldn't control its own guardrails. This week, the same principle showed up in the security workflow itself.

The agent can find the issue. It can help prove the issue. It can propose a fix. It can even help test the fix.

It shouldn't be the only thing deciding that the issue is resolved.

## This changes the economics of security work

The part I'm still working through is what this means for ordinary development teams.

Most of us aren't reverse engineering operating-system binaries or running hundreds of security agents in parallel. We do have codebases with old features, configuration options that have accumulated over time, shared libraries, and security checks that aren't always applied where we think they are.

That makes some of these techniques useful well before we reach the scale of the research projects.

I can ask an agent to inventory what runs before authentication. I can ask it to identify combinations of settings that change a security boundary. I can have a separate review process look beyond the changed file and into the components that depend on it. I can test the result against the application's actual behavior instead of accepting a convincing explanation.

Collectively, I think we need to treat all types of automation, from simple scripts through to complex processes, with the same rigor we would bring to a multi-tiered application. That includes the automation reviewing our security.

AI is going to make vulnerability discovery cheaper. It is probably going to make exploitation cheaper too. The useful question isn't whether we can keep that capability out of the world. It is whether defenders can build it into their normal engineering work quickly enough to benefit from it.

I'm interested in what happens when security review becomes something we can run continuously instead of saving for a periodic assessment. I'm even more interested in making sure the review remains independent from the agent that produced the change.

Would you let the same agent write the code, test the exploit, approve the fix, and tell you the system is safe?
