---
title: PoshMongo
description: "PowerShell cmdlets for working with MongoDB databases, collections, and documents, implemented with C# and the MongoDB driver."
excerpt: "A bridge between PowerShell workflows and MongoDB document operations."
portfolio_group: earlier
order: 1
category_label: PowerShell and data tooling
technologies: ["PowerShell", "C#", ".NET", "MongoDB.Driver"]
resources:
  - title: "PoshMongo repository and cmdlet documentation"
    url: https://github.com/PoshMongo/PoshMongo
---

## Overview

PoshMongo provides PowerShell cmdlets for connecting to MongoDB and working with databases, collections, and documents.

## Motivation

For an administrator already working in PowerShell, database operations are more useful when they fit the same command and object workflow as the rest of the automation.

## Engineering approach

The implementation uses C# cmdlets and the MongoDB driver to expose connection, query, insertion, and removal operations. The repository includes command-specific documentation, so consumers can discover the operations through the PowerShell interface.

## My contributions

This is part of my earlier PowerShell and .NET tooling work. My contributions include MongoDB driver and .NET updates, handling for the `-Force` parameter, and cmdlet documentation.

## Current status

The public repository remains available and was not marked archived when checked on October 23, 2026. That does not establish a maintenance schedule or compatibility with every current MongoDB and PowerShell version.
