---
title: XML2Doc
description: "Turning C# compiler XML documentation into repeatable, linkable Markdown through a library, CLI, and MSBuild integration."
excerpt: "Deterministic API documentation generation with shared rendering and output planning across .NET development workflows."
portfolio_group: featured
order: 4
category_label: Developer tooling
technologies: ["C#", ".NET", "XML documentation", "Markdown", "MSBuild"]
resources:
  - title: "XML2Doc repository and documentation"
    url: https://github.com/mod-posh/xml2doc
  - title: "YAML2Doc"
    url: /projects/yaml2doc/
---

## Overview

XML2Doc converts C# compiler XML documentation into Markdown. It exposes the same work through a reusable core library, a command-line tool, and an MSBuild package so documentation can be generated as part of the build.

## Motivation

API comments are close to the code, but compiler XML is not a convenient reference for someone browsing a repository. Generating readable pages also raises practical problems: stable links, predictable file paths, and knowing which generated files a tool owns.

## Engineering approach

The core handles parsing, rendering, and output ownership. The CLI supports repeatable invocation, while MSBuild connects generation to compilation. Single-file and per-type output serve different documentation sizes.

The public 2.4.0 documentation describes shared output planning for paths, links, manifests, and writes. It also covers caller-supplied metadata, namespace-folder layouts, collision detection before writing, and extension points for rendering. Keeping those decisions consistent matters when documentation from several projects shares an output tree.

## My contributions

My work includes CLI option and configuration validation, along with review and integration of changes. The project reflects a recurring interest of mine: making documentation part of the engineering workflow rather than a separate manual copy of it.

## Current status

The public repository documents the 2.4.0 feature set as of October 23, 2026. Its README contains the current configuration and compatibility details; this page is an engineering overview rather than an installation guide.
