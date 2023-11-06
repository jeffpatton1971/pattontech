---
layout: post
title:  "Creating a report in Jira"
date:   2023-11-03 16:27:00 -0600
categories: blog
tags: November 2023 Development Jira
author: Jeff
comments: true
published: true
---

I've been doing a lot lately with ChatGPT, nothing terribly exciting and I think I'll write a post about that at some point, most likely next year, big plans and all. Anyway, I needed to remember this for the next time I need one of these so here we are. I asked ChatGPT how to create Release Notes in Jira and it gave me this, sadly the exporting aspect did not work, but this may be a limitation imposed by my company.

# Summary: Generating Report for Done Bugs and Tasks in a Specific Epic

## Steps

1. **Navigate to the Epic**:

   - Open the epic you're interested in by clicking on it from the backlog or board view.

2. **Create a Filter for Done Tasks and Bugs within the Epic**:

   - Click on the "Create Issue Filter" icon (looks like a funnel) at the top right of the epic window.

   - In the filter creation window, use the following JQL query:

     ```JQL
     "Epic Link" = EPIC-1234 AND issuetype in (Task, Bug) AND status = Done
     ```

     Replace `EPIC-1234` with the key of your epic.

   - Click "Save As" to save this filter with a descriptive name like "Done Tasks and Bugs in Epic".

3. **Use the Filter in a Board or Search**:

   - Go back to your board or use the search functionality in Jira.

   - Apply the filter you just created by selecting it from the list of available filters.

4. **Exporting the Report**:

   - Once you have applied the filter and can see the list of done tasks and bugs, locate the export option (typically represented by an icon with an arrow pointing outwards or the word "Export").

   - Select the format you prefer for your export (e.g., CSV for a spreadsheet, PDF for a document).

   - Save the file to your preferred location on your computer.
