---
layout: post
title:  "JSON, Strings and Constructors"
date:   2022-09-29 22:18:00 -0600
categories: blog
tags: September 2022 Development
author: Jeff
comments: true
published: true
---
I don't know how other folks work, but I tend to work in blocks of time and occasionally those happen to line up with my working hours, but that's not always the case. The day before I had this idea that it would be really cool if I could re-construct an object from the output Json. For the record, I'm talking about the build automation stuff I've been working on. In the last post I talked about how instead of working against the data I decided to embrace it and leverage DataSets and DataTables. This gave me the ability to easily cast a DataRow as a given cloud resource and have access to the methods and properties of the object as defined in the Schema.

So, why not the other way? Up to this point the constructors were all just sort of there:

```c-sharp
public GenericResource()
{}
```

Now I did add constructors to the resources so I could create them from raw json. That actually grew out of my personal testing to make sure things were working properly, but it seemed like a convenient feature so I left in. Which got me thinking why not construct the entire build from Json. Some of the reasoning behind this comes out of the work I've started on the Framework (the tooling that convers the JSON into TF). If we pass in an object it's a little easier to work with instead of having to serialize/deserialize it.

For the resources I just pass in a JsonDocument, serialize it into the resource and just iterate over the properties to populate them, it seems a little hacky, but it's working. This method however did not work for passing in an entire json file, or as I came to realize all my empty constructors were the issue. I didn't realize this at the time, so I hacked away at the project on and off all day. The issue I had was, once the json is stored in a JsonDocument, or JsonNode, or JsonElement, or JsonObject it's not terribly easy/convenient to pull the properties/values out, or at least I was really struggling with that.

The solution I worked out was to pass in a Json String, convert it into a JsonDocument then grab the first JsonElement and simply pass it down the line:

```c-sharp
public Build(string Json)
{
    JsonDocument document = JsonDocument.Parse(json);
    Platform = new Platform(document.RootElement.GetProperty(nameof(Platform)));
}

...

public Platfrom(JsonElement element)
{
    Azure = new Azure(element.GetProperty(nameof(Azure)));
}
```

That's pretty straightforward and I follow that all the way down until I get to the resources and for the time being I've just left ths JsonDocument constructors as is. But the interesting thing was this all came to me at 1a the night/morning before, which back to what I was originally talk about, this is often how things work for me. I find this particular solution rather elegant, it's perhaps not the best, but it works really well.

The idea literally woke me up, and it was rather simple, pass the element down to each thing as you go. You're basically walking down a filter and you can see it in the raw Json if you play with it, but each GetProperty() returns the properties below where you're currently at in the Json, so you don't have to worry about finding the particular resource above where you currently are, it's already been filtered out!

I should be at a point after today where I can start incorporating a lot of this into my MultiCloudDeployment project.

Thanks for sticking around, and have a great weekend!