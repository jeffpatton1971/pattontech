---
title: llamarc42
description: "A documentation framework for keeping engineering decisions and project knowledge useful across people, tools, and AI development sessions."
excerpt: "Project-owned documentation, explicit authority, and deterministic structural validation for human and AI-assisted engineering."
portfolio_group: featured
order: 3
prominent: true
category_label: Documentation and AI-assisted engineering
technologies: ["Markdown", "C#", ".NET", "Markdig", "Architecture Decision Records"]
resources:
  - title: "Framework documentation and decisions"
    url: https://github.com/llamarc42/docs
  - title: "Core discovery and validation library"
    url: https://github.com/llamarc42/core
  - title: "CLI roadmap and repository"
    url: https://github.com/llamarc42/cli
  - title: "Engineering Intent series"
    url: /series/engineering-intent/
  - title: "InfrastructureIntent"
    url: /projects/infrastructureintent/
---

## Overview

llamarc42 is a documentation-first framework for organizing authoritative project knowledge. Its purpose is to keep architecture, constraints, decisions, and conventions available to both people and AI coding agents without tying that knowledge to a particular model or service.

## Motivation

Remembering what we were working on is not the same as remembering why we chose an approach. Long conversations helped me develop ideas, but they were a poor place to keep decisions that the next development session needed to respect.

I wanted the project itself to hold that context. Documentation should remain useful when the developer, coding agent, or tool changes.

## Engineering approach

The framework defines how a project declares its documentation root and distinguishes current authority from proposals and historical material. ADRs preserve decisions; repository conventions and agent-neutral instructions give contributors shared context. People remain responsible for the substance and acceptance of that documentation.

The C# core implements local discovery, inventory, and structural validation for Documentation Contract v0.1. It produces ordered diagnostics and explicit valid, invalid, or error outcomes without modifying the project or fetching linked URLs.

This is deliberately a bounded form of validation. Checking document structure does not establish that an architectural decision is correct. Nor does organizing documentation require an inference service, agent orchestrator, or hosted knowledge store.

## My contributions

I have been developing the documentation-first approach described in the Engineering Intent articles and carrying it into the framework. My implementation work includes core validation and review fixes that align document and ADR checks with the accepted contract.

## Current status

As reviewed on October 23, 2026, core discovery and structural validation are implemented with fixture tests. Independent validation work remains separate. The user-facing CLI v0.1 is planned and is not yet implemented; older local-copilot specifications in the repositories are historical material, not the current product boundary.

## Related writing

The reasoning is developed in [Four Principles for AI-Assisted Development]({% post_url 2026-10-08-four-principles %}), [Documentation Preserves It]({% post_url 2026-10-15-documentation-preserves-it %}), and [Governance Protects It]({% post_url 2026-10-23-governance-protects-it %}).
