---
layout: post
title:  "Active Directory Naming Strategy"
date:   2010-09-20 19:25:00 -0600
categories: blog
tags: September 2010 KU SOECS
---
Does anyone have a good naming convention for Active Directory? It’s all completely subjective, what works for one organization is absolutely horrible for the next. Does anyone document what their naming convention is? Perhaps a better question would be, if you document it, do you follow it? How many people are aware of the 15 character limit on Window’s NetBIOS hostnames? How many know that limitation is still painfully applicable?

What are the reasons for having a naming convention? Answering this may help you determine one that works for you. If you want to be able to physically locate a machine based on it’s name that will lead you to a completely different convention than if you just wanted to track asset information. In our case, we wanted to move away from the previously undocumented convention. It seemed to a combination of the previously mentioned examples.

Computers that were created under the previous convention were named something akin to the following:

> DEPT-Room-Station

That kind of worked, but we currently manage computers 4 buildings so there was no way to know what building a given machine was physically in. We could figure out which buildings it wasn’t in, based on the number of floors in each, but as you can guess this didn’t work well. We needed something that a brand new employee could translate easily without having to know what building and floor each of the departments happened to be in.

What we settled on was relatively simple:

> Building-Room-Station

Building is a one-letter designation for any of the four buildings we have computers in:

* Burt – B
* Eaton – E
* Learned – L
* Regents – R

The room number is between 3 and 5 characters long. This gives us the ability to have an office that is 123 or 1123A. Finally Station, that is typically a 4 character code that determines what computer in a room of several, this particular computer is. It also differentiates between a laptop and desktop, which originally I was opposed to, but really wound up working out well later.

* Laptop 3 – LT03
* Desktop 31 – PC31

So, for example the third laptop in the second floor office 2100-A of Burt Hall would have the following name:

> B2100a-LT03

It’s pretty simple to figure out, it doesn’t reference anything that we don’t care about and fits nicely inside the 15 character NetBIOS limit.

But let’s be honest, does that really matter? Sure, it works wonderfully when the computer is initially setup but around here, computers have acquired locomotion! This year B2100a-LT03 might actually be in that office, but maybe the professor gets moved to an office on the 4th floor of Learned. Does his computer magically stop working because it’s in the wrong room? Don’t you wish! Of course not, that computer will continue to chug along it’s merry way, never knowing it did anything more interesting than a reboot.

So the real problem I think is enforcement. Which around here always seems to be the real problem. How do you enforce a naming convention around here? Well, we decided that the easiest way is to not worry about it. Computers move, when we receive a support request from B2100a-LT03 we will walk over to Burt, walk into that office and see the computer is no longer there.

In this situation, ideally, the tech would then contact the owner and find out what office they are currently located in. The technician would then check Active Directory to see if the new name is already taken. If it is, we simply increment the station number. So as part of the service call, the technician will logon using her admin account, rename and reboot the computer. Problem solved and we can move on with our life.
