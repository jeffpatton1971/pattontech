---
layout: post
title:  "Github Actions and Ramifications of moving"
date:   2024-01-13 08:26:00 -0600
categories: blog
tags: January 2024 Development WIR
author: Jeff
comments: true
published: true
---

The majority of this week was spent yet again waiting for others, but the good news is I think I may be on the home stretch of my actual work. Honestly I was dead on accurate in the amount of time I dedicated to the work I needed to get done, I did not account for the holiday stretch between November and December, on top of that folks taking time at the end of the year. Needless to say this won't happen again, and really to add to that, I will start to slot in other work when I'm forced to wait for others. Since November this has all been a very long ride on the struggle bus, and I will not be taking that ride again!

In other more pertinent news I have begun to start the conversion from my local psake environment over to Github Actions. My first stab was writing the in Python, while not terribly complex, that's a muscle I don't really want to stretch, so I've decided to re-write them in PowerShell. This is in line with what I typically write in for my personal and business code. Since I'm starting to consolidate all that code into the [Mod-Posh](https://github.com/mod-posh) Organization, I decided to move them from my personal repositories to that org.

My assumption was that I could unlist the Github Action and then publish it from the new repository. This was incredibly inacurate on my part, what needs to actually happen is to just transfer the repository. When you transfer the Github Action repository to a new owner/organization it updates everything for you on the back-end. Long story short, I didn't do that for the first one, I've now been on a Github Support journey, where the action can't be published because of a naming conflict, but when you search for that name in the marketplace it doesn't exist.

```text
Name must be unique. Cannot match an existing action, user or organization name.
```

I am not aware of the backend process, but my guess is there is a database entry someplace that contains the name of my action and it's all messed up. So in order to prevent you from facing this issue, please just transfer the repo, that's what I've done with the remaining Github Action repos and no issue at all.

Once things have been sorted I plan on writing about the actions and how to set them up and use them. For the most part these will mimic my current workflow which may or may not be yours, but hopefully I will have built enough flexibility in so that you can also use them either all together or individually.

Have a great week!
