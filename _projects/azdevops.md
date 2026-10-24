---
title: AzDevOps
description: "Earlier PowerShell automation for Azure DevOps organizations, projects, teams, and builds through the REST API."
excerpt: "Reusable PowerShell commands around Azure DevOps service APIs."
portfolio_group: earlier
order: 2
category_label: PowerShell infrastructure automation
technologies: ["PowerShell", "Azure DevOps REST APIs", "PlatyPS", "psake", "PSScriptAnalyzer"]
resources:
  - title: "AzDevOps repository and command documentation"
    url: https://github.com/Azure-Devops-PowerShell-Module/AzDevOps
  - title: "Historical module reference"
    url: /projects/azdevops/reference/
  - title: "ADO CLI — newer command-line work"
    url: /projects/ado-cli/
---

## Overview

AzDevOps is earlier PowerShell tooling for working with an Azure DevOps organization through its REST API. Its documented commands cover projects, teams, builds, logs, and related operations.

## Motivation

Repeated service operations are easier to reuse when authentication and request handling sit behind named commands. This project brings those operations into PowerShell scripts instead of requiring each script to construct them from scratch.

## Engineering approach

The project organizes API areas into modules. PlatyPS supports command help, psake automates parts of the module and README generation, and PSScriptAnalyzer checks PowerShell code conventions.

The historical documentation describes further test and pipeline work as planned. That planning text is not evidence that those capabilities were delivered.

## My contributions

My work includes module documentation, external help, and review of changes. It represents the earlier PowerShell side of the developer tooling I continue to work on today.

## Current status

The repository was public and not marked archived on October 23, 2026. Its documentation does not establish a current maintenance commitment. The newer ADO CLI is a separate project with a focus on diagnostics and structured command-line workflows.
