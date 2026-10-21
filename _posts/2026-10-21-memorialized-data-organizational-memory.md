---
layout: single
title: "Memorialized Data: Can AI Reconstruct an Organization's Memory?"
date: 2026-10-21 08:40:00 -0500
categories: blog
tags:
    - October
    - "2026"
    - ArtificialIntelligence
    - ContextEngineering
    - KnowledgeManagement
    - SoftwareArchitecture
    - AIAssistedDevelopment
    - CustomerGovernance
author: Jeff
comments: true
published: true
---

I was recently having a conversation with a technology leader when he brought up something he referred to as **"memorialized data."**

I'd never heard that term used in the context of AI.

Memorializing something is common in the legal field. You memorialize an agreement, a conversation, or a decision by creating a record of it. In technology, we're more likely to hear terms like *institutional memory*, *organizational memory*, or perhaps *enterprise RAG* when discussing how to retrieve that information.

The problem he was describing is pretty straightforward, and it's incredibly common, particularly in large, established organizations.

Organizations have years of information about their customers, projects, decisions, and implementations, but that information is spread across systems that were never necessarily designed to work together.

It's not unusual for an established organization to have multiple ticketing systems, several documentation platforms like SharePoint and Confluence, conversations spread across Slack, Teams, and Outlook, multiple CRM systems, and perhaps an internal sales database or two.

And those are just the systems we know about.

Each system serves a purpose. Sales needs to track opportunities and customer relationships. Architects need to document solutions. Engineers need tickets and implementation details. Support teams need operational history.

But none of those systems necessarily provides a complete picture of the customer.

The information exists. Finding it, understanding how it relates, and determining whether it's still relevant is another matter.

What caught my attention was how different this is from the way I typically approach documentation.

## Documentation from the Other Direction

Most of my recent engineering work has taken a documentation-first approach.

Before implementing something, I want to understand what we're trying to accomplish. What problem are we solving? What are the inputs and outputs? What constraints exist? What architectural decisions have already been made?

I want that information documented so engineers, and increasingly AI agents, have a reliable source of context when working on a project.

I've spent quite a bit of time exploring this through my work on engineering intent, architectural decision records, and documentation governance.

The general approach is to establish the knowledge first, maintain it as the project evolves, and make it available to the people and tools doing the work.

But what happens when you inherit years of customer history where nobody was deliberately maintaining that knowledge in one place?

The information may still exist. It's just scattered across several systems, often without any real structure connecting it.

Instead of starting with documentation and using it to guide implementation, we're starting with historical information and trying to reconstruct the knowledge that should have been documented.

I hadn't really considered approaching documentation this way before.

## The Customer Journey Nobody Documented

Consider a typical customer engagement.

A salesperson starts working with a prospective customer. There are emails, meetings, proposals, and eventually some kind of agreement.

Some of that information makes its way into a CRM system.

The engagement moves into planning. Architects meet with the customer, discuss requirements, evaluate options, and make decisions.

Some conversations happen in Teams or Slack. Others happen through email. Maybe a meeting was recorded and transcribed. Perhaps an architect created a design document.

Eventually, the project moves into implementation.

Engineers receive tickets, create infrastructure, build pipelines, troubleshoot issues, and deploy the solution. Support teams take over, and more information accumulates as the environment evolves.

Months or years later, someone asks:

**Why was this customer's environment designed this way?**

The original architect might remember. Someone might find an old email explaining the decision. Maybe there's a ticket documenting a technical constraint or a meeting transcript where the customer rejected an alternative.

But nobody necessarily knows where to look, and the information may span several different systems.

We probably have a reasonably complete history of what happened, assuming the records still exist. What we don't necessarily have is an explanation of why.

And that information isn't just useful to engineers.

The account team might need to understand why a customer made a particular investment. A salesperson might want to identify an opportunity to expand an existing service. Support might be investigating recurring issues that started months before anyone recognized a pattern.

All of them are looking at the same customer, just from different perspectives.

## Could an Agent Reconstruct That History?

My initial thought was that if all this information existed in one system, we could point an AI agent at it and, with the right instructions, potentially generate something useful about the customer engagement, from the initial conversations through deployment.

Of course, it's not all in one system.

And even if it were, simply giving an agent access to everything wouldn't necessarily produce useful results.

So how would we approach this?

The first challenge is collecting the information.

We'd need to identify which systems contain relevant records and establish some way to access them. Email, CRM, project management, chat, document repositories, meeting transcripts, and perhaps deployment systems.

That's primarily an integration problem.

Once we have access to the information, though, we need to determine what's actually useful.

An email thread might contain twenty messages, but only two of them explain an architectural decision.

A meeting transcript might contain an hour of conversation, but the relevant information could be three minutes where an architect explains why a particular approach won't work.

A project ticket might describe what was implemented without explaining why it was implemented that way.

This is where I think an LLM could be useful during ingestion.

Language models are pretty good at interpreting unstructured text. We could potentially use an agent to identify customers, projects, decisions, technical requirements, constraints, and relationships between records.

Rather than simply indexing documents, we could begin extracting information and organizing it into something we can work with.

For example, an agent might identify:

- A customer requirement discussed during an initial meeting.
- An architectural option proposed in an email.
- A technical constraint identified by an engineer.
- A decision made during a subsequent meeting.
- A project ticket implementing that decision.
- A deployment record showing when the implementation occurred.

Individually, those are just records.

If we can establish how they relate to one another, we might be able to reconstruct how a particular decision was made and what happened as a result.

## Reconstructing the Timeline

One of the first problems I'd want to solve is chronology.

If we're trying to understand how a project evolved, the order of events matters.

Initially, this seems pretty easy. Most systems have timestamps.

But what does the timestamp actually represent?

An email might be forwarded weeks after the original conversation. A project ticket might be created after work has already started. A meeting transcript could be uploaded several days after the meeting. A design document might be revised without preserving the original decision.

We would need to distinguish when information was recorded from when the actual event occurred.

We'd also need to account for changes over time.

Imagine an architect recommends one approach in January. The customer agrees in February. During implementation in March, an unexpected constraint forces the team to choose a different approach.

If an agent retrieves only the February agreement, it may report a decision that was later superseded.

So we need more than a timeline of documents. We need to understand how the requirements, decisions, and implementations changed over the course of the project.

I'm not sure a collection of document summaries would be enough to accomplish that.

## Separating the Good from the Fluff

The next challenge is relevance.

Not every conversation deserves to become organizational knowledge.

Anyone who has worked on a large project knows how much noise accumulates. Meeting invitations, status updates, repeated questions, abandoned ideas, incomplete discussions, and conversations that never result in decisions.

Some of that information might be useful historically, but much of it probably shouldn't influence an agent answering a question about a customer's current environment.

I think we could do quite a bit with LLM-assisted ingestion here.

Rather than feeding everything into a retrieval system and hoping the right information comes back, we could have an agent evaluate records as they're discovered.

Is this a requirement? A proposal? A decision? An implementation detail? A constraint? Is it related to an existing customer or project? Does it supersede something previously recorded?

We could give the agent a defined set of classifications and have it extract structured information from the source material.

We would still need to validate that output, of course.

**An LLM can identify information that appears significant. That doesn't necessarily mean the information is correct or authoritative.**

An engineer suggesting a change in Teams is not the same as an approved architectural decision.

A salesperson describing a customer requirement isn't necessarily the same as a signed statement of work.

And a deployment completing successfully doesn't prove that the infrastructure matches the customer's original intent.

I've run into a similar problem when working with infrastructure automation.

A successful Terraform plan tells us that the configuration can potentially be deployed. It doesn't tell us whether we're deploying what the customer actually requested.

We can retrieve information and generate an answer, but we still need some way to establish whether that answer is correct.

## Where Does Authority Come From?

This is one of the harder problems I see with this approach.

In documentation-first engineering, I try to establish authority deliberately.

We have architectural decisions, constraints, specifications, and implementation guidance. Those documents tell an engineer or agent which information governs the project.

Historical discussions provide context, but they shouldn't automatically override approved decisions.

With memorialized data, that authority may never have been explicitly established.

An agent might find several conflicting accounts of the same decision.

Which one wins?

The newest? The one written by the architect? The one mentioned most frequently? The one that appears to match the deployed infrastructure?

I don't think there's a universal answer to that.

We could potentially have the agent identify what the records appear to establish, preserve references to the original sources, and indicate where information is contradictory or incomplete.

It could distinguish between a proposed decision, an approved decision, and an implementation that appears to have followed that decision.

Where authority can't be established, the system should be able to tell us that.

And perhaps certain conclusions should require human review before becoming part of an authoritative customer knowledge base.

I'd be much more comfortable with a system that can explain where its information came from and identify uncertainty than one that simply produces a convincing summary.

## What Would the Architecture Look Like?

I haven't built a system like this, but I've been thinking about how I might approach it.

I'd probably start by separating the ingestion process from whatever we're going to use to retrieve and evaluate the information.

We'd need connectors to the systems holding the data. Those connectors would have to preserve source identities, timestamps, customer associations, and access permissions.

Next, we'd need some form of normalization.

Email messages, CRM records, meeting transcripts, and project tickets are all structured differently, but many of them describe similar concepts.

We could define models for customers, projects, requirements, decisions, constraints, and events, then use an agent to help extract that information from the source material.

That sounds relatively straightforward until we start dealing with relationships.

Does this email refer to the same project as that ticket? Is this customer name an alias for an existing account? Does this decision supersede an earlier one? Is the information relevant to the current environment or a previous implementation?

We'd need some way to establish those relationships without losing the original source information.

I'd also want every extracted conclusion to be traceable to the records supporting it.

From there, we could begin thinking about how agents would consume that information.

Maybe someone wants to know why a particular architectural decision was made. Maybe an engineer needs to understand the history of an inherited environment. Perhaps a project manager wants to understand how a customer requirement changed during implementation.

Each of those questions might require different information, even though they're all drawing from the same historical records.

We also have to consider security.

Just because an agent can retrieve information doesn't mean every user should be allowed to see it. Customer boundaries, permissions, sensitive information, and retention requirements would need to be preserved throughout the process.

That becomes particularly important when combining information from systems with different access models.

At this point, I don't think I'd describe the solution as simply RAG.

RAG might be part of how we retrieve information, but we're also dealing with chronology, identity, relationships, authority, and governance.

There would be quite a bit of conventional software engineering involved before an agent could reliably answer questions using the collected information.

## Starting with What We Know

As I've been thinking about this, it occurs to me that I'd probably approach this the same way I approach most engineering projects.

I'd start with a conversation.

Pick a customer we know reasonably well and start working through what we already know about them. How did the engagement begin? What were they trying to accomplish? What decisions were made along the way? What did we eventually build?

Then I'd start asking where that information came from.

Maybe we know about the original requirements because an architect remembers the conversation. Perhaps the implementation details are in a project management system. Maybe someone knows that a particular decision was made because of a customer constraint documented in an email.

We could start identifying the information we'd need and where we'd expect to find it.

From there, I'd almost certainly create a repository and begin documenting what we're learning.

This is actually pretty similar to how [InfrastructureIntent](https://github.com/infrastructureintent) evolved.

That project started with a conversation about what I knew from building an infrastructure automation platform, what I wanted to preserve, and what I didn't want to carry into a complete rewrite. We started defining the problem, establishing constraints, and documenting the architecture before building the implementation.

I don't see why this problem would need to start differently.

At its core, this is a familiar engineering problem. We have multiple inputs, a set of requirements, and an output we're trying to produce.

The inputs happen to be customer information scattered across different systems instead of infrastructure specifications.

We still need to define our models, establish validation rules, understand dependencies, and decide what a successful result looks like.

We don't necessarily need to design an entire organizational knowledge platform before we understand what information we're trying to recover.

Start with one customer. Work through a known history. Identify the sources, relationships, and gaps. Then figure out how much of that process we can automate.

Once we have something working, we can begin expanding it to other customers and projects.

And there's another possibility here.

If we can establish a framework for reconstructing historical customer information, perhaps we can use that same framework to begin capturing information as new engagements progress.

A salesperson has information about the initial opportunity. An architect understands the requirements and design decisions. Engineers know what was implemented and why. Support teams accumulate knowledge about what happens after deployment.

Instead of waiting several years and trying to reconstruct all of that, we could begin collecting and organizing it as the engagement evolves.

It wouldn't necessarily have to happen in real time. Maybe records are processed periodically, or when a significant event occurs.

We'd be establishing a consistent way to preserve customer and project knowledge across the different teams involved.

We'd still need to work through authority, access controls, and how information gets reviewed. But we'd have a foundation to build on rather than starting from scratch every time someone needs to understand a customer's history.

At that point, we're no longer just reconstructing organizational memory.

We're beginning to establish a way to maintain it.

## From Customer History to Customer Governance

I can see this becoming a framework for establishing governance around customer information.

Not governance in the sense of adding another approval process or forcing everyone into a new system.

I'm thinking about defining how customer knowledge is collected, organized, validated, and maintained so that people across the organization can make better decisions.

An engineer looking at a customer's infrastructure needs a different view of that information than a salesperson evaluating an opportunity.

An architect might want to understand why a particular technology was selected. A support technician might need to identify recurring operational issues. An account manager might want to understand whether the customer is satisfied with the services they're receiving.

They're all working with the same customer, but each has different questions.

If we had a common framework for maintaining customer knowledge, we could begin answering those questions using information collected throughout the customer relationship.

And potentially, we could begin identifying things nobody thought to ask about.

Consider customer health.

Many organizations use some variation of a green, yellow, and red status to describe the condition of a customer relationship.

Green means things are going well. Yellow suggests concerns are developing. Red means there's a significant problem that needs attention.

But how do we know when a customer is moving from green to yellow?

Maybe there's an increase in support tickets. Maybe the customer has raised the same concern in several meetings. Perhaps an important project milestone has slipped, or an architect has identified a technical issue that hasn't been resolved.

Individually, those signals might not seem particularly significant.

But if we could connect information from support, engineering, sales, and customer communications, an agent might identify a pattern before someone recognizes that the relationship is deteriorating.

That doesn't mean I'd want an LLM independently deciding that a customer is now yellow or red.

I'd want the organization to define what those statuses mean, which signals matter, and how they're evaluated. An agent could help collect evidence, identify potential concerns, and surface them for review.

We could apply the same thinking to sales opportunities.

Maybe a customer has repeatedly discussed capacity limitations. Maybe support tickets indicate they're outgrowing an existing solution. Perhaps an architect documented an expansion requirement six months ago that never became a formal opportunity.

With the right context, an agent might help identify opportunities for the account team to investigate.

There are obvious considerations around permissions, privacy, and which information should be used for which purpose. We'd need to define those boundaries rather than assuming every piece of customer information should be available to every team.

But the broader idea seems useful.

Instead of treating customer information as something each department maintains independently, we could establish a governed view of the customer that supports decisions across the organization.

The framework wouldn't replace the CRM, ticketing systems, or documentation platforms. Those systems would continue doing what they were designed to do.

We'd be building a way to connect the information they contain and establish how that information should be interpreted.

## The Connection to Engineering Intent

This conversation stuck with me because it relates closely to what I've been exploring in my [Engineering Intent series](https://patton-tech.com/series/engineering-intent/).

I've been working on ways to preserve the purpose, constraints, architecture, and decisions behind engineering work so future developers and AI agents don't have to rediscover them.

The idea is to document what we're doing and why we're doing it as part of the engineering process.

Memorialized data approaches the same general problem from the other direction.

The work has already happened. The decisions may have been made years ago. The knowledge exists, but often as disconnected pieces of information.

Now we're trying to recover the reasoning behind those decisions.

With documentation-first engineering, I'm trying to preserve what we know so it remains useful.

With this approach, we're trying to recover what was known when nobody preserved it in a way that's easy to consume.

I can also see how these approaches might work together.

Perhaps an agent could help reconstruct historical project knowledge, identify gaps and contradictions, and propose structured documentation that could then be reviewed and maintained going forward.

Instead of repeatedly reconstructing the same information, we could use that initial effort to establish a more useful source of project knowledge.

And if we establish a framework for capturing that knowledge as new customer engagements evolve, we may not need to reconstruct nearly as much of it in the future.

That gets us closer to what I've been trying to accomplish with engineering intent, just starting from a different place and applying it to a much broader organizational problem.

## A Different Way to Think About AI

We spend a lot of time talking about what AI can generate.

Code, infrastructure, documentation, plans, reports.

I've spent a considerable amount of time working in those areas myself.

But this conversation had me thinking about using AI to understand what already exists.

Organizations have spent decades accumulating information. Much of it is technically accessible, but it can be difficult to use because it was never organized around the questions people would eventually need to ask.

Using agents to connect historical information, identify decisions, and reconstruct context seems like a useful application of the technology.

And if we can establish a framework for maintaining that knowledge, the possibilities extend beyond simply answering questions about what happened.

We could support engineering decisions, identify gaps in customer implementations, recognize sales opportunities, and potentially catch developing customer issues before they become larger problems.

But I wouldn't want to build a system that simply turns scattered information into confident-sounding answers.

I'd want to know where those answers came from, whether the information is still valid, what might contradict it, and whether it represents an actual decision or just a conversation.

There are quite a few engineering problems to solve before we get there.

**Recording information gives us history. Connecting it gives us context. Establishing its authority is what makes that context trustworthy.**

I don't have a complete solution to this problem, and I'm sure there's more complexity here than I've covered.

But I know how I'd begin approaching it.

Start with a customer we know. Understand the information we have, identify what we're missing, and establish a framework for working with it.

Then build from there.

Maybe some of the more useful applications of AI won't involve generating something new at all.

Maybe they'll involve finally making sense of the information we've been collecting all along.
