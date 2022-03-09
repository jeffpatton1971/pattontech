---
layout: post
title:  "Microsoft Interviews"
date:   2022-03-09 12:03:00 -0600
categories: blog
tags: March 2022 Review
author: Jeff
comments: true
published: true
---
So, I applied for a Senior Software Engineering Role with Microsoft, specifically with the Deployment Team that handles the Azure Resource Manager Deployment engine. I guess I made it through the screening interview and this week had the technical interviews and let me tell you...WHOO...was rough! VERY ROUGH! lol

First of all, I really enjoyed talking with each interviewer. It was just a general treat to just talk with them and regardless of what happens I've enjoyed the overall experience.

## First Interview

The interviewer was a PHd, the first guy, a P H D! Anyway he was very cool and the interview part was nice, but the coding part wow.... So I thought I would attempt to prepare but I was nowhere near what I thought they would ask. Given a list of Resources with Dependencies, please output the Deployment chain, keep in mind a dependency may have dependencies...YIKES...I took too much time and while I was proud of what I had it was nowhere near complete.

``` c#
public class Resource {
 public string Name {get;set;}
 public Resource[] Dependencies {get;set;}
}

public static Resource CalcDepencies(Resource Resource) {
 Resource Value = new Resource();
 foreach (Resource Dependency in Resource.Dependencies) {
  if (Resource.Depencies != null) {
   Value = CalcDependencies(Dependency);
  }
 }
 Return Value;
}
```

I liked that part of it, the problem was I feel I made it too complicated. If they pass in a string of values 'a,b,c,d' where a depends on b and c depends on d etc, I maybe should have come up with something simpler. For my benefit he did say this was a non-trivial task, so I did feel a little better about that. What impressed me about him was his answer to why does he enjoy working at Microsoft and he said working there he has a global impact and he enjoys the responsibilty of that.

## Second Interview

The second interview was with a manager and again, super cool and just a pleasure to talk to. Yet again, I feel like I bombed out on the technical part and here I think I was making it overly complex. Can the incoming string be turned into a palindrom by removing at most one letter.

``` c#
public static void Palindrome (string word) {
 int length = word.Length;
 char first = new char();
 char last = new char();
 int count = 0;

 do {
  first = word.ToCharArray()[count];
  last = word.ToCharArray()[(length - (count +1))]
  count ++;
 } until (first == last)
}
```

So, I got stuck at this point if you take the word 'tacocat' we're good through 'tac' but count now sits on the letter 'O'. If I don't break out of this we're going to keep iterating down the line until we run out and overlow the array. Like the previous, I feel like I was maybe 25% of the way there. Anyway as he was a manager I asked about a time where he felt like he really helped an engineer that was reporting to him. He said he enjoyed helping his engineers reach the next level of their position and have the opportunity to tackle larger and more complex problems. He felt that his experience as a Software Engineer for several years allowed him to share his experiences so others could learn from him, I thought that was really cool.

## Third Interview

This interview didn't really have a code-writing component, it was more of a problem solving question. You need to change the in-flight movie streaming from a popular service to something in-house. It needs to be able to continue working even with no internet as well as have options for a free and paid-service. Fortunately I didn't have to figure out infrastructure or codecs or streaming technologies, was more of a high-level view. My solution was to store movies on the plane and stream directly over the plane's wifi. Paid customers would have the option at ticket purchase to select movies as well as a possibility during flight, being cognizant of the fact if you swipe a credit card and there is no way to validate the purchase you won't get to watch the movie. Then there was how to sync the service such that all planes in the fleet hvae the same movices. I suggested that when the plane lands part of it's service might include hooking it up to the network to sync, or he suggested the idea of swapping drives. Something akin to moving large amounts of data from on-prem to the cloud, store it on a disk and ship it.

## Last Interview

This interview was with the person who originally screened me and he was delightful. As we had talked at length already most of the time was spent on the coding problem he presented. Basically it was a change tracking problem, you receive a payload of installed software, check to see if anything has changed. What I was thinking was basically you have a process that polls for installed software, if there is no checkfile create one and exist. If there is a checkfile you will need to compare the new poll against the previous. He was more interested in how I approached the diffing part of the problem.

``` c#
public class SoftwareTrack {
    public string time;
    public string name;
    public List<Application> Software;
}
public class Application {
    public string name;
    public string version;
}
public class Output {
    public string time;
    public string name;
    public string description;
}

public static Output DiffSofware(SoftwareTrack Checkpoint, SoftwareTrack NewCheck)
{
    Output Result= new Output();

    Result.time = NewCheck.time;
    Result.name = NewCheck.name;

    if (Checkpoint.time == NewCheck.time) {
        return null;
    } else {
        foreach (Application mine in Checkpoint.Software) {
            Application Current = NewCheck.Software.FirstOrDefault(x => x.name == mine.name);
            //
            // check updated
            //
            if (mine.version != Current.version) {
                Result.description = "Updated " + mine.name + " " + mine.version + ">" + Current.version;
            }
            //
            // Check exists
            //
            if (Checkpoint.Software.Contains(Current)) {
            } else {
                Result.description = "Installed " + Current.name + " " + Current.version;
            }
        }
    }
    return Result;
}
```

This was close, the only problem I had was figuring out if something had been uninstalled perhaps with 15 more minutes or so I would have sorted that out as well. I don't think I'll be offered this position but it was a very rewarding experience for me, and I truly enjoyed interviewing. I think for this level of job I'll need to do a lot more work on my own programming, but we'll see how it goes.
