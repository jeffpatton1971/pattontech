---
title: Jira CLI
description: "A Jira Cloud command-line tool for repeatable issue searches, updates, and workflow actions with explicit automation boundaries."
excerpt: "Jira Cloud workflows through jcli, with typed service operations, structured results, and separate credential adapters."
portfolio_group: featured
order: 7
category_label: Developer workflow automation
technologies: ["C#", ".NET 10", "System.CommandLine", "Jira Cloud REST APIs", "Markdig"]
resources:
  - title: "Jira CLI repository and coverage matrix"
    url: https://github.com/jeffpatton1971/jira
  - title: "ADO CLI"
    url: /projects/ado-cli/
---

## Overview

`jcli` brings Jira Cloud workflows to the command line. It supports JQL searches, issue work, transitions, comments, worklogs, and board and sprint queries for people and automation.

## Motivation

Development work frequently crosses the boundary between a repository and an issue tracker. Repeatable queries and explicit issue operations let that work fit into a terminal workflow without rebuilding Jira HTTP requests in each script.

## Engineering approach

Command parsing and presentation live in the CLI. A core library owns results, errors, redaction, and limited Markdown-to-Atlassian Document Format conversion. The Jira Cloud layer owns endpoints, transport, pagination, and service behavior. Native credential access sits behind separate platform adapters.

Those boundaries let the executable stay focused on the user's action while the service layer handles Jira-specific behavior. The tool targets Jira Cloud and JQL; it does not claim Jira Data Center support or federated natural-language search.

## My contributions

My work includes the command surface, Jira Cloud client, credential providers, core safety behavior, and cross-platform contract tests. I also worked on workstation documentation and macOS Keychain's first-use authorization. Making installation and authentication understandable is part of making a CLI useful.

## Current status

The reviewed repository has implemented commands and a coverage matrix. As of October 23, 2026, its README states that no package or executable release has been published; installation is from a locally built tool package.
