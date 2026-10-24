---
title: ADO CLI
description: "A .NET command-line tool for investigating Azure DevOps pipeline failures, collecting selected build evidence, and inspecting packages."
excerpt: "Azure DevOps Services diagnostics and bounded automation through the ado command-line tool."
portfolio_group: featured
order: 6
category_label: Developer workflow automation
technologies: ["C#", ".NET 10", "Azure DevOps Services REST APIs", "JSON"]
resources:
  - title: "ADO CLI repository and capability matrix"
    url: https://github.com/jeffpatton1971/ado
  - title: "AzDevOps — earlier PowerShell tooling"
    url: /projects/azdevops/
---

## Overview

`ado` is a standalone CLI for Azure DevOps Services. Its focus is practical investigation: finding a pipeline run, inspecting its timeline and logs, collecting selected artifact evidence, and checking feeds and package versions.

## Motivation

Troubleshooting a build often means piecing together information from several screens and service APIs. A command-line interface makes those steps repeatable and gives scripts and coding agents structured results they can inspect.

## Engineering approach

The solution separates CLI presentation, application behavior, domain models, service clients, and platform credential access. Pipeline runs, builds, classic releases, and packages remain distinct concepts rather than being flattened into one generic request interface.

JSON output and explicit error handling support automation. Bounded downloads and selected evidence keep investigation manageable. Read-only diagnostics are the priority; retained pipeline actions have dry-run and exact-confirmation controls.

## My contributions

My work includes the CLI foundation, review fixes for safety boundaries, and opt-in live integration checks. Platform verification is recorded separately from compilation so a successful build does not imply that every native credential path has been exercised.

## Current status

As reviewed on October 23, 2026, the README describes a 1.0.0 release candidate with publication pending. Some native-platform and live-service verification remains open. It targets Azure DevOps Services, not on-premises Azure DevOps Server.
