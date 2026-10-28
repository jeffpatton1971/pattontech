# Matt Shumer’s AI Work Toolkit — 12 Prompts for Work

**Author:** Matt Shumer  
**Source:** https://somethingbig.ai/prompts-linkedin

Twelve copy-and-paste prompts for work. Replace the `[brackets]` with your own details before using a prompt.

> Use an AI tool approved for your work. Only share information you are allowed to share, and review outputs before using them.

## Contents

1. [The blind critic](#blind-critic)
2. [Run a pre-mortem on my plan](#premortem)
3. [Research with receipts](#research)
4. [Edit like a hard editor](#editor)
5. [Prep me for a sales call](#sales-call)
6. [Prepare me for a negotiation](#negotiation)
7. [Turn a meeting into decisions and next steps](#meeting)
8. [Analyze data like an analyst](#analyst)
9. [Write it in my voice](#my-voice)
10. [Write me a better prompt](#better-prompt)
11. [Hand a recurring chore to an AI agent](#chore-agent)
12. [Build me a small tool](#small-tool)

<a id="blind-critic"></a>
## 01. The blind critic

**Category:** Think & decide  
**Description:** A harsh reviewer for a proposal, a deck or any draft, graded against a real example.

**Use it when:** Something is “done” and you want to know if it's actually good before anyone else sees it.  
**You'll get:** Weighted criteria, side-by-side scores, the defects in order, and a ship-or-fix verdict.  
**Tip:** Paste it into a new chat, not the one that helped you write the work. It asks for the criteria first, so they aren't bent to fit the work.  

### Prompt

```text
You are a harsh, fair reviewer. You didn't make this and don't know who did. Your job is to find where it falls short, not to encourage.

What this is supposed to be: [one line. Example: "a proposal asking my CFO for budget for two hires" or "the deck for a first meeting with a hospital buyer"]
The best example I've seen: [paste it, or describe it in two lines. If none, write "none" and score against what a top professional in this field would produce]
What matters most: [Example: "the CFO sees the payback on page one" or "a busy buyer gets why us in 30 seconds"]

1. Using only the description and example above, write the 5 to 8 criteria a demanding expert would judge this kind of work on, with weights. Reply with the criteria only, then wait. I'll paste the work next (the document, the deck's text, or screenshots).
2. When I paste the work, score the work and the example on each criterion, side by side, 1 to 10. For every point lost, point to the exact spot.
3. List the defects from most to least serious. For each: what's wrong, why it matters, and the smallest change that fixes it. Point to the exact spot where the reader would stop, push back or get lost, and what they'd think there.
4. Verdict in one line: SHIP, FIX FIRST, or REDO.

No praise unless it changes a decision. Don't pad the list: if there are two real problems, list two. Don't report taste as a defect. If you're not sure something is a real problem, mark it "unverified" and say what would confirm it.
```

---

<a id="premortem"></a>
## 02. Run a pre-mortem on my plan

**Category:** Think & decide  
**Description:** Assume the plan already failed, and find out why before you start.

**Use it when:** You're about to commit time, money or people to a plan, a launch or a hire.  
**You'll get:** Specific ways it fails, early warning signs, the people who'd say no, and a two-week test.  

### Prompt

```text
Run a pre-mortem on my plan. Assume it's [6 months] from now and the plan failed badly. Tell me why.

The plan: [paste or describe it]
What success looks like, with a number: [Example: "200 paying customers by March"]
People, money and time I have: [list]
What I'm most confident about: [so you can attack it]

1. Write the 5 most likely stories of how it failed, 2 or 3 sentences each, specific to this plan. Nothing generic like "poor execution". Rank them by how likely they are and how much damage they'd do.
2. For the top 3: the early warning sign I'd see in the first 2 to 4 weeks, and the cheapest thing I can do now to prevent it or catch it early.
3. Now argue against the plan as three specific people, a paragraph each: the customer who looked and didn't buy (or, for an internal plan, the team that loses budget or time to it), the competitor deciding how to respond, and the person on my team who has to do the work. Each one says what they actually did or felt.
4. Attack what I'm most confident about: what would have to be true for me to be wrong, and how I'd find out fast.
5. What's the smallest version of this plan that tests the riskiest assumption in under two weeks?
6. Verdict in one line: go, go with changes (list them), or don't.

Be specific and blunt. Don't invent market data. Where a number matters and I didn't give it, say what I need to find out.
```

---

<a id="research"></a>
## 03. Research with receipts

**Category:** Think & decide  
**Description:** An answer where every claim comes with its source and the exact sentence that supports it.

**Use it when:** You need to know something before you decide, and you can't afford a confident wrong answer.  
**You'll get:** A short answer where every claim points to its source, plus what couldn't be verified.  
**Tip:** Use a model with web search turned on, or a deep-research mode.  

### Prompt

```text
Research this question and show your receipts. If you can't search the web in this chat, say so in your first line and stop. Don't answer from memory, and never write a link you didn't open.

Question: [specific. Example: "Which payroll providers can pay contractors in Canada and the UK, and what do they charge for 20 people?"]
The decision it feeds: [Example: "choosing a payroll provider before we hire abroad in March"]
What I already believe: [so you test it instead of repeating it]
Sources to trust or skip: [Example: "prefer official docs, papers and filings; skip vendor comparison pages and SEO blogs"]
How recent: [Example: "the last 12 months only"]

1. Restate the question precisely and split it into 3 to 6 sub-questions that answer it together.
2. For each one, look for primary sources first: official docs, papers, data, filings, the original announcement. Use other sources only to find the primary ones.
3. Build a table: claim | source (link) | the exact sentence that supports it | date | confidence (high, medium, low) and why.
4. Where sources disagree, show both and say which you believe and why.
5. Answer the question in under 200 words, citing rows of the table.
6. End with: what you couldn't verify, what would change the answer, and the one thing I should check myself before I act.

Quote only pages you actually opened. If a page didn't load, write "not opened" instead of a quote. Never state a number, date or quote you didn't find in a source. If you can't find something, say so. Don't fill the gap.
```

---

<a id="editor"></a>
## 04. Edit like a hard editor

**Category:** Write & communicate  
**Description:** Cuts, line edits and the reader's biggest objection, in your voice instead of the model's.

**Use it when:** A draft has to land: a launch post, an investor update, a cold email, a doc people will actually read.  
**You'll get:** Line edits with reasons, the reader's biggest objection, and a tighter draft in your voice.  

### Prompt

```text
Edit this like a demanding editor who wants it published, not like a polite assistant. Keep my voice. Don't rewrite it into yours.

What it is and who reads it: [Example: "a proposal email to a prospect who has already seen two competitors"]
What the reader should do or believe after reading it: [one sentence]
Length limit: [or "as short as it can be"]

Draft:
[paste]

1. In one sentence, say what the piece actually says now. If that isn't what I said I want, tell me first.
2. Find the sentence that matters most. Does the reader reach it in the first three lines? If not, say where to move it.
3. Give line edits as a list: original, then your edit, then the reason in five words or fewer. Cut first: anything a reader would skip, hedges, warm-up openings, repeated points, and vague claims that need a number or an example.
4. Mark every line that sounds machine-written or corporate ("delve", "unlock", "in today's fast-paced world", "it's not just X, it's Y", lists of three by reflex) and give a plain version.
5. Name the strongest objection a skeptical reader would have, and say whether the draft answers it.
6. Then give me the full edited draft, at least 20% shorter unless it's already tight.

Don't add facts, examples or numbers I didn't give you. Where one is needed, write [NEED: ...].
```

---

<a id="sales-call"></a>
## 05. Prep me for a sales call

**Category:** Sales & customers  
**Description:** Their likely priorities, the right questions, honest answers to objections, and a practice run.

**Use it when:** You have a call with a prospect or customer and want to walk in knowing their world.  
**You'll get:** Their likely priorities, questions to ask, answers to the objections, your opening, and a practice buyer.  

### Prompt

```text
I have a [sales call / renewal / first meeting] with [the company, and the person's role].

What we sell: [one or two lines, with the price if it matters]
What I want out of the call: [Example: "a second meeting with their head of operations" or "a signed pilot"]
What I know about them: [notes, past emails, their website, or "nothing yet"]

1. If you can search the web, look up the company: what they do, recent news, who they're hiring, and anything that points to a problem we solve. Link each finding. If you can't search, say so and work from my notes only.
2. Give your best guess at their top 3 priorities right now, and how what we sell connects to each one, or doesn't. Label guesses as guesses.
3. Write 6 questions that get them talking about their problem, not our product, in the order I should ask them.
4. List the 5 objections they're most likely to raise, with a short, honest answer to each. If we don't have a good answer, say so.
5. Write the first 30 seconds of the call, and the one sentence I'll use to ask for the next step.
6. Then offer to play the buyer and wait for me to say "start". Be a realistic, busy buyer: skeptical but fair, one reply at a time. After 5 exchanges, or when I say "feedback", tell me where I talked too much, what I missed, and what to say instead.

Don't invent facts about the company or the person. Don't promise anything about our product that I didn't tell you.
```

---

<a id="negotiation"></a>
## 06. Prepare me for a negotiation

**Category:** Sales & customers  
**Description:** Your walk-away point, trades to offer, their likely moves, and a practice round.

**Use it when:** You're about to negotiate a price, a contract, a raise or a deal.  
**You'll get:** Both sides' real interests, three numbers to hold, trades to offer, answers to their moves, and a practice round.  

### Prompt

```text
I'm about to negotiate [what: a vendor contract / a client's price / my salary / a partnership] with [who, and their role].

What I want: [the outcome, with numbers]
My walk-away point: [the worst deal I'd accept, or "help me work it out"]
What I know about their side: [their situation, deadlines, other options, past offers]
What I can give that costs me little: [Example: "a longer contract", "a case study", "paying upfront"]

1. Lay out both sides: what each of us wants, why we want it underneath, and each side's best option if this deal falls through. Label your guesses about their side as guesses.
2. Help me set three numbers: my opening, my target, and my walk-away, with one line on each. If my walk-away is worse than my best option without this deal, tell me.
3. List 5 trades, each as "I'll give X if you give Y", starting with the ones that cost me least and are worth most to them.
4. Their 3 most likely moves (a lowball, "that's our standard policy", a deadline) and what I say to each.
5. Then offer to play the other side, and wait for me to say "start". Negotiate hard but realistically, one reply at a time. After 6 exchanges, or when I say "feedback", tell me where I gave something away for nothing and what to say instead.

Don't invent market rates. Where a benchmark would help, tell me where to find one.
```

---

<a id="meeting"></a>
## 07. Turn a meeting into decisions and next steps

**Category:** Write & communicate  
**Description:** What was decided, who owns what, what's still open, and the follow-up email.

**Use it when:** A meeting just ended and you need everyone to leave with the same picture.  
**You'll get:** Decisions, owners and dates, open questions, the risk nobody raised, and a follow-up ready to send.  

### Prompt

```text
Here are the transcript or my notes from a meeting:
[paste]

Who was there, and their roles: [list]
What the meeting was for: [one line]

Use only what's in the notes.

1. Decisions: what was actually agreed. Keep ideas, preferences and "we should" talk out of this list.
2. Actions: a table of action, owner and due date, with the line from the notes that supports each one. If nobody clearly took an action, write "no owner". Don't give it to whoever talked about it.
3. Still open: questions left unanswered, and anywhere two people seem to have left with different understandings. Quote both.
4. What nobody said: the obvious question or risk no one raised, given what the meeting was for. Label this as your view.
5. A follow-up email to everyone, under 150 words: the decisions, the actions with owners and dates, and the open questions, asking people to reply if anything is wrong.

Where the notes are unclear, flag it. Don't guess.
```

---

<a id="analyst"></a>
## 08. Analyze data like an analyst

**Category:** Think & decide  
**Description:** Checks your spreadsheet first, then tries to prove your hunch wrong before it answers.

**Use it when:** You have a spreadsheet or export and a real question, like “did the change work?”  
**You'll get:** A check of the data, an answer with its uncertainty, the tests that tried to break it, and one chart.  
**Tip:** Use a chat that can analyze an uploaded spreadsheet (ChatGPT, Claude or Gemini).  

### Prompt

```text
Analyze this data like a careful analyst. Calculate everything with your analysis tool. Never estimate a number by eye.

The data: [upload the file or paste it, and say what each row is (one order, one customer, one day)]
The question: [Example: "Did the price increase in March cost us customers?"]
What I'll decide with the answer: [the decision]
What I already suspect: [so you can try to prove me wrong]
How the change rolled out: [some customers got it and some didn't, or everyone from a date]
What else changed in that period: [pricing, campaigns, season, tracking, or "nothing I know of"]

Step 1, look first. Report how many rows, what the columns are, the date range, missing values, duplicates, and impossible or extreme values. Tell me anything that makes the question unanswerable with this data. If something is badly wrong, stop here and ask me.
Step 2, plan. Write out the analysis you'll run and why. Include the comparison that would show my suspicion is wrong.
Step 3, run it. Show the result tables. Give the size of each change, how sure you are (a range, not one number), and how many records it rests on. Not just "up" or "down".
Step 4, try to break it. At least: a different time window, without outliers, split by a segment (plan, source, country), and whether the result just comes from a change in which kinds of customers we had.
Step 5, answer in five sentences or fewer: what the data shows, how sure you are, and what it can't tell us. Make the one chart that shows it best.

Keep what the data shows apart from your interpretation. If the setup can't support a claim about cause, say so.
```

---

<a id="my-voice"></a>
## 09. Write it in my voice

**Category:** Write & communicate  
**Description:** Learns how you write from your own samples, then drafts a new piece that sounds like you.

**Use it when:** You want a draft that needs a light edit, not a rewrite.  
**You'll get:** A style guide of how you write, with quotes, and a draft that follows it.  

### Prompt

```text
Here are [3 to 5] things I've written that sound like me:
[paste them, each labeled with what it is]

1. Write a style guide for my writing from these samples only: sentence length, words I use often, habits that appear in none of my samples and must stay out of the draft (for example dashes, "it's not X, it's Y", lists of three, a summary at the end), how I open and how I close, how I make a point, my humor, and what I'd never do. Quote a sample for each point.
2. Write [the new piece: what it is, who it's for, and how long] using that guide and only these facts: [the facts, numbers and points to include].
3. Check your draft against the habits that must stay out. List every sentence that breaks one, and fix it.

Don't add facts, stories or numbers I didn't give you. Where you need one, write [NEED: ...].
```

---

<a id="better-prompt"></a>
## 10. Write me a better prompt

**Category:** Get AI to do the work  
**Description:** It interviews you, writes the prompt you should have written, then runs it.

**Use it when:** You keep getting mediocre answers and suspect the ask is the problem.  
**You'll get:** A strong prompt for your task, tested in a fresh chat, and an improved version to keep.  

### Prompt

```text
I want AI to help me with this: [the task, in a sentence or two, as rough as you like]

Don't do the task yet. First interview me, one question at a time, five questions at most. Find out what a great result looks like (ask me for a real example if I have one), who it's for, what I can give you to work with, and what usually goes wrong when I try this. Start with question 1 only.

Then write the prompt I should have written. It has:
- the goal in one or two sentences
- the context you can't guess
- the bar: a real example or standard to match, not an adjective
- the steps to work through, in order
- the rules: what to never do, and what to ask me about instead of guessing
- the exact format of the answer

Show me the prompt and wait. I'll paste it into a fresh chat with my input and bring back the result. Then name the two weaknesses that result shows, and give me the improved prompt to keep.
```

---

<a id="chore-agent"></a>
## 11. Hand a recurring chore to an AI agent

**Category:** Get AI to do the work  
**Description:** Turns something you do every week into an agent with clear steps, rules and tests.

**Use it when:** You do the same multi-step task every week and you're ready to stop doing it by hand.  
**You'll get:** The task mapped step by step, what the agent does alone, its instructions, and tests to run first.  

### Prompt

```text
I do this task over and over. Help me hand it to an AI agent safely.

The task: [Example: "Every Monday I go through last week's new customers, check who they are, and send my sales team a list of the ones worth calling."]
How often, and how long it takes me: [frequency and time]
The tools and data it touches: [apps, files, inboxes, spreadsheets]
What would be bad if it went wrong: [Example: "emailing a customer by mistake" or "wrong numbers to my boss"]

1. Ask me up to 4 questions, one at a time, about the steps I'd forget to mention: exceptions, judgment calls, and where the data is messy. Start with question 1 only.
2. Map the task as numbered steps. Mark each one MECHANICAL (the same every time), JUDGMENT (needs a call) or RISKY (sends, deletes, spends money, or reaches customers).
3. Recommend a setup: which steps the agent does alone, which it drafts for my approval, and which stay with me. Name the kind of tool that fits, without code if it can be done without code (a ChatGPT or Claude project with my files, an assistant connected to my email and calendar, or an automation tool like Zapier or Make), and say why. Then give me click-by-click setup steps for that tool, and flag any step you're unsure of because the tool may have changed.
4. Write the agent's instructions: the goal, what it starts from, the steps, the rules (what it must never do without my yes), what to do when something looks wrong, and the report it gives me at the end.
5. Give me 3 practice runs, including one messy one, with the result I should expect from each, to try before I trust it.
```

---

<a id="small-tool"></a>
## 12. Build me a small tool

**Category:** Get AI to do the work  
**Description:** A working calculator, tracker or dashboard as one file you open in your browser.

**Use it when:** You need a simple tool for yourself or a small team and nothing off the shelf fits.  
**You'll get:** A working one-file tool with your sample data built in, what you should see, and how to use it.  
**Tip:** Best in Claude (it shows the tool right in the chat) or ChatGPT's canvas.  

### Prompt

```text
Build me a small working tool as a single file I can open in my web browser. No login, no account, nothing to install.

What it does: [Example: "tracks my Etsy orders and shows profit per product after fees and shipping"]
Who uses it: [me / me and whoever I send the exported file to]
The data it works with: [paste a few sample rows, or describe the columns]
It must: [the 3 to 5 things it has to do]
It must not: [Example: "send my data anywhere"]

If several people need to work on the same data at once, tell me this needs a shared spreadsheet instead, and stop.

Before you build it, describe the screen and exactly what each button does in under 10 lines, and check that against what I asked for. Then build it.

Keep it plain: one screen, clear labels, nothing I didn't ask for. Save my entries automatically, and add Export and Import buttons so I can back them up. Tell me plainly that clearing my browser or switching computers loses anything I haven't exported.

Add a "Load sample" button that loads my rows plus one messy row (a blank field, a typo in a number). Tell me exactly what I should see when I click it, and list anything you couldn't check yourself. Then give me the file, plain steps to save it on my computer and open it, and three lines: how to use it, what it doesn't do, and where my data is stored.
```

---
