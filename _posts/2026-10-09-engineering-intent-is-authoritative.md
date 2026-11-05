---
layout: single
title: "Engineering Intent Is Authoritative"
date: 2026-10-09 08:00:00 -0500
series: engineering-intent
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

One of the things I've always liked about working with AI is that it feels like a conversation.

I don't particularly want an assistant that just agrees with everything I say. Quite the opposite, actually. I've spent a fair amount of time trying to get AI to push back when it thinks I'm heading in the wrong direction.

If I'm making a bad architectural decision, overlooking something, or overcomplicating a problem, I'd rather have that pointed out than have an AI enthusiastically help me make it worse.

And to be fair, I'm perfectly capable of heading down the wrong path all on my own.

That was one of the things I started noticing pretty early on. We'd be making progress on something, and then suddenly we'd be working on something slightly different. Not necessarily something wrong, just not quite what we'd originally set out to do.

Sometimes the AI would introduce an idea we hadn't discussed, or revisit a decision we'd already made. Sometimes I'd get interested in a tangent and we'd both head off in that direction.

The problem wasn't always the quality of the work. It was keeping the work pointed in the right direction.

## It Worked Until It Didn't

When I first started using AI for development, I didn't really understand much about context windows or how all of that worked.

But I noticed something.

If I stayed in the same conversation, things generally went better. We'd already talked through decisions, explored ideas, and figured out what worked and what didn't. Starting a new conversation meant having to explain a lot of that all over again.

So I stayed in the same conversation.

And stayed.

And stayed.

Eventually, those conversations got enormous. Things slowed down, finding something we'd discussed days earlier became a chore, and at some point starting a new chat became unavoidable.

Which meant explaining everything again.

It worked until it didn't.

## Trying to Keep Things Moving

BAT, the build automation platform I was working on, gave me plenty of opportunities to experiment with ways around this.

One thing we tried was building out detailed sprint plans. We'd break the work into cards, document what needed to happen, and export those plans as XML so I could bring them into a new conversation.

That actually helped quite a bit.

Instead of starting from scratch, I could give the AI a reasonably detailed picture of what we were working on.

We also started creating handoffs at the end of a day or week. What did we finish? What were we in the middle of? What were we planning to do next?

Those helped me just as much as they helped the AI.

But we were still missing something.

The sprint plans were good at explaining what needed to be done. The handoffs were good at explaining where we'd left off.

Neither was particularly good at preserving all the decisions we'd made along the way, especially the reasons behind them.

## Getting It Out of the Conversation

One of the bigger improvements came when ChatGPT gained the ability to work directly with my GitHub repositories.

Now we had somewhere to put that information.

We started building out documentation around the architecture, the decisions we'd made, and the conventions we wanted to follow.

Instead of hoping the AI remembered why we'd made a decision three weeks earlier, we could point back to something we'd written down.

The conversations were still important. That's where we explored ideas, argued about approaches, and figured things out.

But the conversation no longer needed to remember everything.

And that made a difference.

## What Happens When Someone Else Shows Up?

For a long time, BAT was really just me and ChatGPT working through things together.

I knew the history. If something got lost between conversations, I could usually fill in the gaps.

But eventually I started thinking about what this would look like if other people were working on it.

What if another developer picked up one of the repositories?

What if someone was using Claude or Copilot instead of ChatGPT?

Would they know where to start? Would they understand why we'd structured something a particular way? Would they even know those decisions existed?

That led to another round of documentation work.

We already had ADRs, development guides, and a project constitution. We started adding things like `AGENTS.md`, Copilot instructions, and other agent-specific guidance to individual repositories.

The idea was pretty simple.

Someone should be able to open any BAT repository, whether they're a developer or an AI agent, and find their way back to the documentation that explains how the project is supposed to work.

Not a separate set of architectural rules for every AI tool. One set of engineering decisions, with instructions that help different tools discover and follow them.

I wouldn't claim we've completely solved that problem. Different agents behave differently, and there's always room for improvement.

But it's a long way from hoping the AI remembers what we talked about last Tuesday.

## So What Does Any of This Have to Do With Engineering Intent?

Looking back, I think I spent a lot of time trying to solve what I thought was a context problem.

And it was, at least partly.

But the bigger issue was figuring out how to keep the work aligned with the decisions we'd made, without getting in the way of challenging those decisions when we needed to.

I still want AI to disagree with me. I want it to point out when something doesn't make sense. I want other developers to feel comfortable doing the same.

What I don't want is for any of us to quietly change the architecture because something seemed like a good idea in the moment.

If there's a better approach, great. Let's talk about it. Maybe we change the plan.

But let's actually make that decision.

**Engineering intent can evolve. It shouldn't drift.**

That's really what I mean when I say engineering intent is authoritative.

And it's what led me toward the next part of this whole idea: using documentation to make sure those decisions don't disappear when the conversation ends.

**Documentation Preserves It.**
