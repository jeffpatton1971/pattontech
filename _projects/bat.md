---
title: "Build Automation Tool (BAT)"
description: "Team engineering at Rackspace: moving from schema-driven Azure builds to a modular .NET infrastructure automation platform."
excerpt: "My work on architecture, resource models, validation, and Terraform generation in Rackspace's internal build platform."
portfolio_group: featured
order: 1
prominent: true
category_label: Professional engineering case study
technologies: ["C#", ".NET", "PowerShell", "JSON", "YAML", "Terraform", "Azure DevOps"]
resources:
  - title: "My Rackspace engineering background"
    url: /about/#rackspace-technology
  - title: "Engineering Intent series"
    url: /series/engineering-intent/
  - title: "InfrastructureIntent — independent infrastructure intent work"
    url: /projects/infrastructureintent/
  - title: "llamarc42 — preserving engineering context"
    url: /projects/llamarc42/
---

## Overview

BAT is an internal infrastructure automation platform I helped design and develop during my time at Rackspace. It was a team effort and is employer-owned software, not an open-source personal project.

The work evolved from schema-driven Azure automation into a .NET platform for processing infrastructure specifications, validating resource models, and generating deployment configurations, including Terraform.

## Motivation

Automating a deployment is useful. Making the whole build process consistent is a larger problem. Different cloud products bring different resource types, defaults, conventions, and requirements. The result should not depend on which engineer happens to run the build.

Earlier in that progression, I developed a JSON build schema and a PowerShell module that consumed build documents to generate deployment templates and supporting documentation. As the work grew, the challenge became finding boundaries that could accommodate more products without putting every product's rules into one engine.

## Engineering approach

The later platform separated core orchestration from provider-specific resource knowledge and output generation. Structured resource models and validation gave the system something more explicit to work with than a collection of templates.

Provider and plugin contracts allowed cloud-specific behavior to evolve separately. Keeping resource models distinct from their Terraform representations also separated what the infrastructure meant from how a deployment tool expressed it.

Repeatable generation needed more than a renderer. Deterministic output, useful diagnostics, automated tests, and build verification made changes reviewable. Architecture Decision Records and engineering documentation preserved the reasoning behind shared contracts as the platform evolved.

## My contributions

My work included architecture, C#/.NET development, PowerShell tooling, provider models, validation, defaults, diagnostics, Terraform generation, testing, and engineering documentation. I also worked on build and release processes and integration across repositories and engineering teams.

Those were contributions to a broader platform built with other engineers. This case study describes the public engineering context without reproducing internal code, documents, diagrams, or customer configurations.

## What I took from the work

The difficult part was not simply generating infrastructure. It was keeping a growing system understandable: deciding where a rule belonged, making failures useful to the person investigating them, and recording why a boundary existed before the next change put pressure on it.

That experience informs my independent work on InfrastructureIntent and llamarc42. They are separate projects with their own designs and ownership boundaries.
