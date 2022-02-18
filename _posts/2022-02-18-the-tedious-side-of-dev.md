---
layout: post
title:  "The tedious side of development"
date:   2022-02-18 08:21:00 -0600
categories: blog
tags: February 2022 Rackspace Development
author: Jeff
comments: true
published: true
---
So I've been working on things both personally and for work. Both projects have similar goals but are implemented differently. But one of the things that I ran up against early on was the idea of defaults. In a [json schema](https://json-schema.org/understanding-json-schema) you can have [default](https://json-schema.org/understanding-json-schema/reference/generic.html) values among a vast array of other options.

> The default keyword specifies a default value. This value is not used to fill in missing values during the validation process. Non-validation tools such as documentation generators or form generators may use this value to give hints to users about how to use a value. However, default is typically used to express that if a value is missing, then the value is semantically the same as if the value was present with the default value. The value of default should validate against the schema in which it resides, but that isn’t required.

The original code was pretty generic and, to be blunt, probably rather naive. This is partly due to how I was working with the existing schema and how I thought the code should work versus the reality. So if we look at a simple example, that may help things along.

```c#
using System;
using System.Collections.Generic;

namespace Aws.Resources
{
    public class Ses
    {
        public Ses()
        {
            ProvisionDkim = false;
        }
        public string Domain { get; set; }
        public string SenderEmail { get; set; }
        public bool ProvisionDkim { get; set; }
    }
}
```

So there are a few things going on here that are worth noting. First, we are setting the default in the constructor, so I assumed that would handle passing in a null/empty value. This was, of course, not true, and I think the reason was because of how these classes were being consumed.  If we were to instantiate this in code or live via PowerShell, it would have that default value, but that's not really how these things are getting created.

So the second attempt we changed a few things around.

```c#
using System;
using System.Collections.Generic;

namespace Aws.Resources
{
    public class Ses
    {
        public Ses()
        {
            ProvisionDkim = false;
        }
        public string Domain { get; set; }
        public string SenderEmail { get; set; }
        private bool? _ProvisionDkim;
        public bool? ProvisionDkim
        {
            get
            {
                if (_ProvisionDkim == null)
                {
                    return false;
                }
                else
                {
                    return _ProvisionDkim;
                }
            }
            set
            {
                if (value == null)
                {
                    _ProvisionDkim = false;
                }
                else
                {
                    _ProvisionDkim = value;
                }
            }
        }
    }
}
```

What I did here was to create a custom getter and setter as well as allow the ProvisionDkim to be nullable (?). The thought was if we pass in a null/empty value that this should allow the getter/setter to evaluate the incoming value and set it accordingly. Again, this should work, but again the way that these are being used I was not getting the results that I anticipated.

So what finally worked?

```c#
using System;
using System.Collections.Generic;
using System.Text.Json.Serialization;
using System.ComponentModel;
using System.ComponentModel.DataAnnotations;

namespace Aws.Resources
{
    public class Ses
    {
        public Ses()
        {
            ProvisionDkim = false;
        }
        public string Domain { get; set; }
        public string SenderEmail { get; set; }
        private bool? _ProvisionDkim;
        [DefaultValue(false)]
        public bool? ProvisionDkim
        {
            get
            {
                if (_ProvisionDkim == null)
                {
                    return false;
                }
                else
                {
                    return _ProvisionDkim;
                }
            }
            set
            {
                if (value == null)
                {
                    _ProvisionDkim = false;
                }
                else
                {
                    _ProvisionDkim = value;
                }
            }
        }
    }
}
```

Here you can see that we are using DataAnnotations and setting a default value. So, why does this work when the other's did not? Well, it has to do with the magical way the code is being called. Honestly it has to do with how I'm using the classes and this is where things differ between work and personal. We are converting classes to json schema, so while that part worked the resulting schema output never had the default values specified. Once the DataAnnotations were added the output schema contained the defaults and when the automation was run against it with live data, we were now getting defaults where values were left purposefully blank.

In all honesty I think that I could remove those custom getter/setter functions as I doubt they would ever actually be used in production and would only serve to confuse people who would have to maintain or attempt to troubleshoot this code in the future.

So, let's get to the tedious because that was a lot of leadup! I've spent the past few days going through each class and adding the DataAnnotations where they are required. I've not yet formally decided if I should remove the custom getter/setter and considering I'm only a third of the way through the entire process and I'm having to take sanity breaks...I may wind up just leaving them in.
