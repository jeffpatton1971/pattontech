---
layout: post
title:  "DataTables, Nulls, Strings, Ints and Casting"
date:   2022-09-17 01:30:00 -0600
categories: blog
tags: September 2022 Development
author: Jeff
comments: true
published: true
---
I've been very focused on my work lately, and it's been gratifying. I've learned a great deal in the past few weeks. So let's recap, I'm working on a solution enabling you to deploy resources into any cloud. At work, this revolves around using terraform, but for the work I do off the clock, I'm hoping to be able to target platform-specific templating languages. The solution at work leverages an Excel Spreadsheet to gather inputs; this is genuinely a non-optimal solution, but, at the time, it's what everyone decided was the correct choice. 

### Ingestion

This process takes an Excel file and converts it into a JSON document. This work went through several different iterations across several various forms of Excel input. The most successful was using the Azure Calculator document. The downside to it was the data was difficult to parse consistently. But it laid the groundwork for what was to come. The process depends on understanding the input, codified into a JSON schema. Building on work I had done in the past, we fleshed out a schema that defined resources across clouds.

To understand the Schema, we needed a way to work with it, and from there, the SchemaModule was born. This module allows us to retrieve data from Excel and lay it out in a regular and repeatable way. So we would collect all the resources from Excel and pass them through the SchemaModule to generate a JSON document that would be the source of truth for a given build. I created a cloud module for each platform, allowing the process to work the same regardless of the cloud.

That work all came together incredibly quickly and worked well for a long time, but it wasn't the best way of doing things. First, you needed to download the Schema, originally a single file. It became too large to process successfully, so the Schema was re-written into multiple files. The idea was you could potentially only pull in the bits you needed. The feature never materialized because I never really implemented that in Ingestion. While this had the potential to be more efficient, you had to download lots and lots of small files, which took time.

For a long time, I had been toying with either reworking the SchemaModule or changing the Schema itself. I started working on a new Schema about a year ago, written entirely in C#. I wrote the namespaces in such a way as to follow the original Schema, so if you had worked with the file-based Schema before, you would have an idea of where things were in the new Schema. This work went through several iterations. The benefit of the file-based Schema was it was easy to add default values and validation rules. 

### DefaultValues

I tried several different methods for handling this, and the first was more current C# code that didn't work in the way I needed it to:

``` c-sharp
public string Sku { get; set; } = "Basic";
```

While this worked, it didn't work the way I had anticipated, I still wanted to be able to generate a schema, and those values were not coming in when I was rendering the Schema out. So we needed to add some decorators:

``` c-sharp
[DefaultValue("Basic")]
public string Sku { get; set; } 
```

That worked well for the schema generation, which was fine in the interim, but as I iterated on the problem, I needed to be able to have more control. I decided that the original Ingestion process wasn't as good as it could be. We were getting a DataTable from Excel and parsing that into custom objects, which was acceptable as a first pass. But I had to go through several hoops to sort out datatypes for each field, and it wasn't always foolproof. Additionally, I had to do a fair amount of looping and using "Where-Object" to get the values I wanted.

I decided it made more sense if, instead of a collection of random resources, we returned a DataSet, where a DataTable defined each set of resources. With a DataSet, I could filter the data I needed more efficiently and in a way that looked cleaner and made more sense for someone coming behind. As a bonus, the Ingestion feels a lot faster than before.

Ingestion will collect each Resource DataTable and append it to a DataSet.Tables collection. I can then run a select filter against a particular Table in the set and get the specific data I need:

``` PowerShell
$BuildSheet.Tables['Disks'].Select("VM = '$($Resource.Name)'")
```

This code returns a collection of disks associated with the VM resource we are currently processing. Before, we had to iterate all resources to find what we were after:

``` PowerShell
$Resources |Where-Object -Property Resource -eq 'Disks' |Where-Object -Property VM -eq $Resource.Name
```

The same data is returned; the difference is one comes in as a collection of DataRow's and the other as a collection of PSCustomObject's.

### Strings, Ints, Null and DBNull

The only problem is getting the proper DataType when defining the DataColumn for the DataTable. In the code describing the column, we read ahead to the next row to see what the DataType of the value is. The only downside is that in the case of DefaultValues, the norm is to leave them blank; this speeds up filling out the spreadsheet. So as a step to sorting that problem, we had to deal with incoming nulls and strings that should be ints. Strings as Ints was an easy one:

``` c-sharp
[JsonNumberHandling(JsonNumberHandling.AllowReadingFromString)]
public int? Instances
```

JsonNumberHandling facilitates this, what is happening at times is the value being read in during column creation is being set as a string, but a subsequent row has a number; this all sounds weird, but I'm getting to the issue. At any rate, this decorator fixed that problem. The real issue is the read-ahead. One solution is to read all values for a given column and evaluate each to determine the proper DataType. The downside is if it's a DefaultValue column, they could all potentially be null. 

The other solution is to grab the Resource from the Schema DLL and determine it that way. This option makes more sense, but when newing up a resource, not all properties have values; the Name of a Resource, for example, is a string, but when you create one from scratch, it's empty it throws an error:

``` PowerShell
$Resource = [Build.Schema.Resource]::new()
$ Resource.Name.GetType()
InvalidOperation: You cannot call a method on a null-valued expression.
```

The solution presented itself as I was writing this, I could add a method to each Resource that would return the type of the property:

``` c-sharp
public Type GetPropertyType (string PropertyName)
{
    Resource r = new Resource();
    //
    // I think reflection needs to happen here, but the pseudo-code looks like this
    //
    // return r.(Propertyname).GetType();
    //
}
```

I could add this to the interface I created or tack it onto each Resource. Then during DataColumn creation, we can ask for the DataType of the property from the Class and be done with it. A lot more elegant, and if I can find a way to make it generic, it could be added to the Resource Interface.

At this point, I started getting errors about DBNull, which got me off on a weird track. I wound up adding a DBNull Converter which I'm 99.8% sure is unnecessary as I think it's already built into .Net. I was testing for null incoming values in the Cloud modules, and once I removed those, the error went away. The only other issue I then ran into was strings as it's. There are a few places where the property is a string value, and a number is entered. Those most likely should be set to its in the Class, but I know that further down the pipeline, other code may be depending on those being strings. So I added a string to int converter; now, if a number comes in as a string, it will work properly.

All these issues will disappear once I have sorted out the DataType for each column.

### Back to Defaults

We're in the home stretch of this post, Ingestion is generating a proper DataSet of DataTables, and those are being processed and output as JSON, but I started to notice I wasn't seeing the values I was expecting. So I modified the getter/setter for defaultValued properties like this:

``` c-sharp
const DefaultSkuName = "Basic";
//
//
//
private string _SkuName;
[DefaultValue("Basic")]
public string SkuName
{
    get => _SkuName;
    set
    {
        if (value == null)
        {
            _SkuName = DefaultSkuName;
        }
    }
}
```

This worked fine when SkuName was left blank. The problem was if you set SkuName (or any property that a defaultValue) to a different value, it wasn't getting processed. That's because I neglected the else side of that test, so I updated all the classes:

``` c-sharp
const DefaultSkuName = "Basic";
//
//
//
private string _SkuName;
[DefaultValue("Basic")]
public string SkuName
{
    get => _SkuName;
    set
    {
        if (value == null)
        {
            _SkuName = DefaultSkuName;
        }
        else
        {
            _SkuName = value;
        }
    }
}
```

Now we're getting somewhere the classes are now much more well defined, and resources with default values have those defined as constants at the top, so if a change is required, it's a simple update in one place that is reflected everywhere.

### Casting

What I ended my day with late Friday was looking at the Cloud modules. These modules are responsible for turning the excel data they are passed into a proper object that can be rendered as JSON and passed along the pipeline to other processes. The process currently is handled by a method on each Class called fromJson(string JSON). When a JSON object is passed in, it is serialized as the ClassType. This works really well, but we need to convert each DataRow into JSON for each call. This generates a warning about JSON depth, which isn't an issue, but if you were running this locally, you might get concerned. 

What if we could _cast_ a DataRow as a Resource:

``` PowerShell
[build.Schema.resource]$DataRow;
```

Or even, what if we could new up a resource with a DataRow:

``` PowerShell
[build.schema.resource]$r = New-Object build.schema.resource($DataRow);
```

There were some fascinating articles and Stack's on this, and what I settled on was a method and ctor for each Class:

``` c-sharp
public static explicit operator Resource(DataRow dataRow)
{
    return new Resource
    {
        Location = dataRow.Field<string>("Location"),
        Name = dataRow.Field<string>("Name")
    };
}
```

This allows me to cast a DataRow as a Resource, which could definitely speed things up. As we define the properties in the Class that is ultimately presented in Excel, it's easy to add them to each Class. Sadly there doesn't readily appear to be a simple way to use this casting in a ctor, so a bit of duplication gets both solutions working for me:

``` c-sharp
public Resource(DataRow dataRow)
{
    Location = dataRow.Field<string>("Location");
    Name = dataRow.Field<string>("Name");
}
```

This makes those cloud modules now a lot more straightforward to understand; initially, they were very confusing. Using classes and data rows cleaned that up a lot, and now being able to cast or new up a resource with a datarow? That will make that code a lot less confusing and more sensible when you look at it.

There's a lot here, and this is really only the first step in a process that will be getting a total overhaul. Not to mention we also need to sort out validation both for the Schema and also for Ingestion. Once that is all sorted, we'll need to make sure the classes all line up once again with the file schema, push all the changes up so that we can transition, and then we can tackle the framework that is generating the terraform.