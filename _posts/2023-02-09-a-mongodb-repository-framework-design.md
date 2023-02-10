---
layout: post
title:  "A MongoDB Repository Framework Design"
date:   2023-02-09 21:02:00 -0600
categories: blog
tags: February 2023 Development
author: Jeff
comments: true
published: true
---
I'm working with MongoDB, if you don't know I've created a nice little PowerShell module, PoshMongo, that allows you to work with MongoDB, you can find it on [Github](https://github.com/PoshMongo/PoshMongo) and the [PowerShell Gallery](https://www.powershellgallery.com/packages/PoshMongo/3.2.1). Mostly this was due to project at work that are using MongoDb and I needed to become more familiar with it. Additionally I've decided to write MongoDB framework, I wanted something that was like the EntityFramework stuff from Microsoft, but instead of a relational database like Sql Server, I wanted MongoDB.

This is what I've been working on this week,  but the articles I found were a few versions old and I needed to sort out some issues. Here are the two articles that I found extremely helpful.

- [MongoDB Repository Implementation](https://www.thecodebuzz.com/mongodb-repository-implementation-unit-testing-net-core-example/)
- [Generic Mongo Repository Pattern](https://medium.com/@marekzyla95/mongo-repository-pattern-700986454a0e)

The first article talked about the pattern and gave some really good examples but for whatever reason I really struggled trying to fit what I'm working with into what the writer had. I think this was more of an issue on my end than the article as it was fairly recent. What I found interesting was the Unit Tests, I need to get into writing those and this may be a very good starting point for me. The second article I was able to wrap my head around, and was ultimately what I used, but I did have a few issues and I'll go over those here for you.

So, I'm working from the second article and when it came out, Asp.net Core projects had a Startup.cs file that was called when the web application started. This changed I believe for .Net 6 and Microsoft posted an [article](https://learn.microsoft.com/en-us/dotnet/architecture/porting-existing-aspnet-apps/app-startup-differences) that touched on it as well as something in Github that I can't seem to find as of this writing. So basically I needed to port the article app over to the current structure.

Basically in order to do this I needed to add a namespace and a Program class, as the article talks about setting up a few classes in order to get things going and you can't have classes just sitting around. This worked out really well, and I've not seen any issues, so if you want to follow along you will need to add a namepsace to your program.cs file then add the two MongoDBSettings classes. The last thing you really need to is then move all the original code from the program.cs inside a static main method, see below:

```c-sharp
namespace DataLayer
{
    public class Program
    {
        public interface IMongoDbSettings
        {
            string DatabaseName { get; set; }
            string ConnectionString { get; set; }
        }

        public class MongoDbSettings : IMongoDbSettings
        {
            public string DatabaseName { get; set; } = string.Empty;
            public string ConnectionString { get; set; } = string.Empty;
        }
        static void Main(string[] args)
        {
         // original code here
        }
     }
}
```

Once you have this layout everything else just slots in nicely. For my own personal benefit I created a Repository directory within my project and then placed the Repository Interface code into an interfaces.cs file and the Repository class itself in the repository.cs file. I created a Models folder and placed my model class in it, and the interfaces that the model is derived from in an interfaces.sc file in the Models directory. Once that was done, all I needed to do was flesh out the controller class, which I placed in the Controllers directory. At this point the project would compile but I needed to make a few changes, the most basic was within the Document interface.

```c-sharp
public interface IDocument
{
    [BsonId]
    [BsonRepresentation(BsonType.String)]
    ObjectId Id { get; set; }

    DateTime CreatedAt { get; }
}
```

The code above was the original, and I struggled with this for longer than I really care to admit. Basically most all the CRUD functions were working as advertised, but everytime I attemped to use the ReplaceOne method as written, I was never getting the document updated in Mongo. I added some breakpoints and peppered in some console output in order to see what was going on and it turns out that my ObjectId value was always 0. I went over and over this several different ways was getting increasinly more frustrated. I finally stumbled upon a [MongoDB Community](https://www.mongodb.com/community/forums/t/working-with-id-in-net/122599/3) post that pointed me in the right direction.

```c-sharp
public interface IDocument
{
    [BsonId(IdGenerator = typeof(StringObjectIdGenerator))]
    string Id { get; set; }

    string CreatedAt { get; }
}
```

I used the IdGenerator and told it to create a string ObjectId, this allowed me to set the Id properly when a document is initially created (which is important) and then be able to query it. That part required a little more effort to realize but was worth it. Below is the original ReplaceOne code, which as written I just could never get to work properly and in the end I chalked this up to my lack of depth in working with the MongoDB driver.

```c-sharp
public void ReplaceOne(TDocument document)
{
    var filter = Builders<TDocument>.Filter.Eq(doc => doc.Id, document.Id);
    _collection.FindOneAndReplace(filter, document);
}
```

My change was rather simple, since I was creating string Id's now, I needed to update the filter to find those.

```c-sharp
public void ReplaceOne(TDocument document)
{
    var filter = Builders<TDocument>.Filter.Eq("_id", document.Id);
    _collection.FindOneAndReplace(filter, document);
}
```

Once I made this change everything clicked and started working, I simply updated the filter in all methods that searched on Id and the entire API just started working. I worked on this for three days and when I hit that, I can't tell you how happy I was to have it finally sorted, and now documented. I need to make a few more changes but I'll publish the code in [MultiCloudDeployment](https://github.com/MultiCloudDeployment).

Currently the application loads the connection string from the appsettings.json file and I'll want it to pull from a Keyvault instead, once I have that sorted the DataAccessLayer should be complete.

Thanks for stopping my and have a wonderful Friday! 
