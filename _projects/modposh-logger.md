---
title: ModPosh.Logger
description: "Reusable console and file logging for C# projects and PowerShell scripts, with configuration and size-based log rotation."
excerpt: "Shared logging behavior for PowerShell and .NET tools."
portfolio_group: earlier
order: 3
category_label: Shared .NET and PowerShell library
technologies: ["C#", ".NET", "PowerShell", "JSON configuration"]
resources:
  - title: "ModPosh.Logger repository"
    url: https://github.com/mod-posh/Logger
  - title: "Historical usage reference"
    url: /projects/modposh/logger
---

## Overview

ModPosh.Logger is a C# logging library that can also be loaded by PowerShell scripts and modules. It provides console and file output, configuration, and size-based file rotation.

## Motivation

Logging is a small concern that appears in almost every automation tool. Sharing that behavior keeps each script from carrying its own variation of message formatting and file handling.

## Engineering approach

The library separates the logging interface, configuration reader, factory, and file utilities. Callers can create console or file loggers directly or use JSON configuration. Information, warning, and error messages share the same implementation.

## My contributions

This project is part of my work on reusable libraries for PowerShell and C#. My contributions include support for multiple .NET targets, build and release automation, and documentation. Those changes let the shared logging code fit into more than one project's build and runtime requirements.

## Current status

The public repository was not marked archived when checked on October 23, 2026. The repository and retained usage reference provide the implementation context; no ongoing maintenance cadence is implied here.
