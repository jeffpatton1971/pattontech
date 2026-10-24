---
title: YAML2Doc
description: "A small YAML-to-Markdown tool with optional GitHub Actions and Azure Pipelines dialects, built around a shared document model."
excerpt: "A predictable documentation view of YAML and CI pipelines, with parsing separated from Markdown rendering."
portfolio_group: featured
order: 5
category_label: Developer tooling
technologies: ["C#", ".NET", "YamlDotNet", "YAML", "Markdown"]
resources:
  - title: "YAML2Doc repository and documentation"
    url: https://github.com/mod-posh/yaml2doc
  - title: "XML2Doc"
    url: /projects/xml2doc/
---

## Overview

YAML2Doc converts YAML into a Markdown document. Its standard dialect provides a simple view of a mapping-root YAML document; optional GitHub Actions and Azure Pipelines dialects expose pipeline details such as triggers, jobs, and steps.

## Motivation

Configuration files are part of the documentation problem too. A readable view helps someone understand what a pipeline contains without maintaining a second description by hand.

## Engineering approach

A loader and dialect registry parse input into a neutral `PipelineDocument`. The Markdown renderer consumes that model, and the CLI owns invocation and file handling. That keeps pipeline-specific interpretation separate from presentation.

The additional dialects are opt-in, preserving the standard YAML behavior. The CLI also bounds file access to the working directory and refuses to overwrite an existing output file.

The tool documents structure; it does not validate pipeline semantics. A readable workflow description is not proof that a CI service will accept or correctly execute it.

## My contributions

My work includes the Markdown renderer, pipeline dialect changes, documentation generation, and build and release workflows. The engineering emphasis is a small, testable pipeline whose default behavior remains stable as optional dialects are added.

## Current status

The reviewed repository documents the v1.1.0 dialect support. It includes tests for the standard behavior and pipeline-aware output. No broader YAML validation or release-download claims are made here.
