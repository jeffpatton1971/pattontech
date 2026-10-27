---
layout: post
title: "Is ChatGPT Really Building a Psychological Profile of Me?"
date: 2026-10-27
author: Jeffrey Patton
categories:
  - Artificial Intelligence
tags:
  - ChatGPT
  - AI Memory
  - AI Governance
  - AI Ethics
  - AI Personalization
description: >-
  A closer look at claims that ChatGPT builds psychological profiles,
  what the underlying research actually found, and why independent
  oversight matters more than sensational headlines.
---

I read an [article in Futurism](https://futurism.com/artificial-intelligence/chatgpt-building-profiles-users) claiming that ChatGPT is building detailed psychological profiles of its users.

Really?

I've been using ChatGPT extensively for software engineering and architecture. We've spent hours working through problems, discussing ideas, and questioning technical decisions.

I'd be surprised if it hadn't learned something about me by now.

What I wanted to understand was why that should be concerning.

## What Does ChatGPT Know About Me?

I decided to ask.

ChatGPT described me as a systems thinker who values architectural consistency, documentation, and reproducibility. It recognized that I question assumptions and prefer understanding why something works rather than simply accepting that it does.

For the most part, it was accurate. Nothing was particularly surprising.

I suspect a colleague who's worked with me for several years could give a remarkably similar description.

Would we say that colleague has developed a psychological profile of me? Of course not. We'd simply say they know how I work.

## What the Research Actually Says

The Futurism article references a paper titled [*The Algorithmic Self-Portrait: Deconstructing Memory in ChatGPT*](https://arxiv.org/abs/2602.01450), presented at the ACM Web Conference 2026.

I wanted to understand what the researchers were studying, so I looked for the original paper.

They examined 2,050 memory entries from 65 ChatGPT users to see how memories are created, what information is retained, and how accurately those memories reflect conversations. They considered potential privacy concerns as well.

They weren't specifically trying to establish whether ChatGPT was building comprehensive psychological profiles.

One finding was that approximately 52% of the memories contained what the researchers classified as psychological information.

The researchers also found that 96% of memory updates happened without users explicitly asking ChatGPT to remember something. That raises a more interesting question for me: how much control should users have over what AI decides to remember?

That sounds concerning until you see how they defined it.

Psychological information included emotions and beliefs, as well as preferences, desires, goals, and intentions. Desires accounted for the largest category.

If I tell ChatGPT I want to build an application in C# using a local language model, remembering that preference could qualify as psychological information under their framework.

I wouldn't call that profiling. I'd call it remembering something I told it.

The researchers documented their methodology and investigated legitimate questions about AI memory. What I question is how those findings were presented in the Futurism article.

The Futurism article wasn't written by one of the researchers. A journalist interpreted their work.

I can't speak to the author's intentions, but there's a big difference between investigating what ChatGPT remembers and claiming it's building detailed psychological profiles of its users.

One makes me curious about the technology. The other seems meant to make me worry.

## An Algorithmic Self-Portrait

I actually think the researchers chose a pretty good title: *The Algorithmic Self-Portrait*.

That's a reasonable description of what ChatGPT has developed about me. It remembers what I've shared, recognizes patterns in our conversations, and notes how I approach problems.

Some of those observations are accurate; others might be incomplete or wrong.

People also use AI differently. I primarily use ChatGPT as a technical collaborator, so most of what it knows about me concerns engineering and professional interests.

Others might use AI more like a conversational companion, even giving it a name. Their conversations might involve relationships, emotions, or personal experiences.

Naturally, the AI would develop a different understanding of those users. Neither representation necessarily describes the whole person.

## Isn't This How We Get to Know People?

We develop impressions of one another all the time.

Coworkers remember how we approach problems. Friends recognize our interests and preferences. Managers form opinions about our capabilities.

Those observations influence future interactions, whether we realize it or not.

I generally know who I'm talking to, but I don't know what information they've retained about me or how their opinions might influence future decisions.

Consider a colleague who challenges a technical decision. You might appreciate that approach, while another finds it frustrating. Both have observed the same behavior but reached different conclusions.

People bring their own experiences and personal preferences into those judgments. Sometimes someone just rubs us the wrong way for no particular reason.

Those impressions can affect opportunities, relationships, and how people treat one another.

AI doesn't have those same personal experiences or motivations. Models can still reflect biases, make incorrect assumptions, and produce unfair outcomes.

But why should we assume human judgment is more reliable?

So I'm struggling to see why AI remembering information and developing an understanding of someone should be considered more concerning than a person doing the same thing.

## Testing AI

We can systematically test how AI systems behave. We can evaluate their consistency, examine whether irrelevant information influences decisions, and see how they handle unexpected situations.

Testing won't uncover every problem. A system could pass thousands of tests yet still fail in ways nobody anticipated.

That's why independent evaluation needs to be ongoing.

What matters is who's doing the testing, what standards they're using, and what happens when a system fails.

## Who Should Be Responsible?

I don't believe AI is inherently safe, and I don't believe companies developing these systems should police themselves.

There are legitimate risks. AI can make mistakes, repeat those mistakes at scale, and affect people who have no idea how a particular decision was reached.

Those concerns deserve serious attention. But I also don't accept the assumption that every new AI capability brings us closer to disaster.

We need to stop treating speculation as evidence and start asking whether these systems meet independently verifiable standards.

OpenAI telling me it has tested its own products and that I have nothing to worry about isn't reassuring.

That's like buying snake oil from a carnival salesman because he assures you it works.

I'm not suggesting AI developers deliberately mislead anyone. Their assurances shouldn't be the final word.

Companies should test their products, but they shouldn't be left to set and enforce safety standards alone.

Other industries have independent oversight for a reason. I don't see why AI should be different.

I expect ChatGPT to remember things about me. That's part of what makes it useful.

I want to know whether those memories are accurate, how they're used, and what happens when the system gets something wrong.

**I don't think we should have to take the developer's word for it.**

---

## References

- [Futurism — ChatGPT Is Building Detailed Psychological Profiles of Its Users](https://futurism.com/artificial-intelligence/chatgpt-building-profiles-users)
- [The Algorithmic Self-Portrait: Deconstructing Memory in ChatGPT](https://arxiv.org/abs/2602.01450)
- [ACM Digital Library — Original Research](https://doi.org/10.1145/3774904.3792671)
