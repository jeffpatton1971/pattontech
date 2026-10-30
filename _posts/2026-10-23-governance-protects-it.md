---
layout: single
title: "Engineering Intent: Governance Protects It"
date: 2026-10-23 08:00:00 -0500
categories: blog
tags:
  - "Engineering Intent"
  - "AI-Assisted Development"
  - "Software Architecture"
  - "Governance"
series: engineering-intent
author: Jeff
comments: true
published: true
---

In the last article, I talked about how documentation became a much more important part of my development process, particularly as I started working more extensively with AI.

We'd gone from relying on long conversations and handoffs to having architectural decisions, development guides, and a project constitution that we could reference whenever we needed to.

That was a pretty big improvement.

But eventually I ran into another problem.

Just because we'd documented something didn't necessarily mean it was being followed.

And that got me thinking about governance.

## Isn't Governance Just More Process?

I'll admit, governance isn't exactly a word that gets me excited about writing software.

It brings to mind approval processes, meetings, committees, and all the other things that can make a relatively straightforward engineering task unnecessarily complicated.

And that's certainly not what I was trying to build.

What I wanted was actually pretty simple.

If we'd already made an architectural decision, I wanted that decision to mean something.

If we'd established that a particular component was responsible for a specific piece of functionality, I didn't want an AI agent casually moving that responsibility somewhere else because it seemed more convenient.

And if we needed to change that decision, I wanted us to recognize that we were changing it.

That doesn't seem particularly unreasonable.

But as it turns out, making that happen consistently takes a little more work than writing everything down.

## The Problem With Reasonable Suggestions

One of the things I've noticed about working with AI is that it can come up with some pretty convincing solutions.

Sometimes they're genuinely better than what I had in mind.

Other times, they're perfectly reasonable solutions to the wrong problem.

That's an important distinction.

An AI agent might suggest adding a dependency between two components because it makes implementing a feature easier.

And in isolation, that might be a perfectly sensible recommendation.

But what if we'd deliberately separated those components because we wanted them to be independently developed, tested, or released?

Now we've got a solution that might work perfectly well, pass every test, and still violate an architectural decision.

The code isn't necessarily broken.

The architecture is.

I've seen variations of this while working on BAT. As the platform evolved, we spent considerable time defining which responsibilities belonged in the core engine, which belonged in providers, and how the components were allowed to interact.

Those boundaries weren't arbitrary. They existed for reasons we'd discussed and documented.

The challenge was making sure those reasons continued to influence the work long after the original conversation had ended.

## From Documentation to Expectations

This is where I started thinking about documentation a little differently.

An ADR explains why we made a decision.

A development guide explains how we expect something to be done.

A project constitution establishes the principles that should guide the overall architecture.

But none of those documents does much good if nobody knows they exist, or if they're treated as optional reading.

With BAT, we started building out instructions for different AI development tools. Files like `AGENTS.md`, Copilot instructions, and other agent-specific guidance helped direct agents back to the central documentation.

The intent was that someone could open a repository, start working with an AI agent, and have a reasonable expectation that the agent would discover the project's architectural rules.

Not because we'd copied every rule into every repository, but because we'd established a path back to the authoritative documentation.

I like this approach because the engineering decisions aren't tied to one particular tool.

ChatGPT, Claude, Copilot, or a local model might all work a little differently, but the architecture shouldn't change depending on which one happens to be writing code that day.

Of course, providing instructions doesn't guarantee they'll be followed.

That's where review and verification become important.

## Trust, but Verify

Something else I've learned is that tests passing isn't necessarily the same as the implementation being correct.

And I don't mean that the tests are bad.

Tests can tell us whether the software behaves as expected under the conditions we've tested. They don't automatically tell us whether we've preserved every architectural decision or respected every dependency boundary.

A successful build doesn't prove that we've built the right thing.

Neither does an enthusiastic summary from an AI agent telling us that everything looks great.

I've found it useful to approach implementation in smaller, more focused pieces, with explicit expectations about what should change and what should remain untouched.

Then we review what actually happened.

Did we solve the problem we intended to solve?

Did we introduce a dependency we didn't want?

Did we change a public contract without recognizing the consequences?

And if something doesn't fit, is the implementation wrong, or have we discovered a reason to reconsider the architecture?

That last question is particularly important.

## Governance Shouldn't Prevent Disagreement

I've mentioned before that I actively encourage AI to disagree with me.

I still do.

And I don't want a governance structure that prevents that.

If an AI agent identifies a weakness in an architectural decision, I want to hear about it. If another developer has a better approach, I want them to bring it forward.

The point isn't to make every decision permanent.

It's to make changes deliberate.

There's a big difference between saying, "I think we should reconsider this architectural boundary, and here's why," and quietly changing that boundary while implementing an unrelated feature.

The first is engineering.

The second is drift.

And again, this applies just as much to me as it does to AI.

I'm perfectly capable of getting excited about an idea and heading off in a direction that doesn't fit what we'd originally agreed upon.

Having those decisions documented gives me something to check my own thinking against.

Having governance around them gives us a way to recognize when we're proposing a change rather than simply carrying out the existing plan.

## Where This Is Leading

One of the things I've been exploring with [llamar42](https://github.com/llamarc42) is how to make this kind of structure reusable.

Rather than rebuilding a collection of architectural documents, conventions, and agent instructions every time I start a project, what would it look like to have a consistent framework for establishing and maintaining them?

And with [InfrastructureIntent](https://github.com/InfrastructureIntent), I've been able to explore what happens when that thinking is applied from the beginning of a project rather than introduced as the project matures.

I'm not looking to create a system where every engineering decision requires a formal approval process.

Quite the opposite.

I'd like the important expectations to be clear enough that developers and AI agents can work independently, recognize when something doesn't fit, and know when it's worth having another conversation.

Because that's really what governance means to me in this context.

It's not about preventing change.

It's about protecting the decisions we've made until we deliberately decide to make different ones.

**Engineering intent is authoritative. Documentation preserves it. Governance protects it.**

Which brings us to the fourth principle, and perhaps the one that started this whole journey in the first place.

**AI Operates Within It.**
