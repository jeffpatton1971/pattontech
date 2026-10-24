---
title: InfrastructureIntent
description: "An independent infrastructure intent compiler project exploring how requirements, domain meaning, and validation survive configuration generation."
excerpt: "Infrastructure intent, typed resource contracts, and layered validation, with public architecture and an evolving implementation."
portfolio_group: featured
order: 2
prominent: true
category_label: Infrastructure architecture
technologies: ["C#", ".NET", "typed resource contracts", "Architecture Decision Records"]
resources:
  - title: "Public architecture and documentation"
    url: https://github.com/InfrastructureIntent/documentation
  - title: "Public extension contracts"
    url: https://github.com/InfrastructureIntent/abstractions
  - title: "Engineering Intent series"
    url: /series/engineering-intent/
  - title: "llamarc42"
    url: /projects/llamarc42/
---

## Overview

InfrastructureIntent is my independent exploration of infrastructure intent compilation: representing requirements, checking their meaning, and preserving that meaning while producing executable configuration.

It has its own architectural boundaries and design principles. It is not a fork, successor, or replacement for BAT.

## Motivation

> A successful Terraform plan is evidence of executability, not correctness.

A deployment tool can accept a configuration without establishing that it satisfies the original requirements. I want the intent and the checks that matter to remain explicit before the work reaches a deployment target.

## Engineering approach

The public architecture explores a compiler pipeline with distinct responsibilities. Adapters read source representations. Integrations supply infrastructure-domain meaning. A resource graph captures identities and relationships. Integration-owned backends translate that graph into target models, and targets validate and emit deployment artifacts.

That separation gives validation several jobs. Domain checks address infrastructure meaning; graph checks address identities and relationships; backend checks address whether a target can represent the requested semantics; target checks address its own model.

The public abstractions already define resource identities, typed references, and contracts for managed and existing resources. A reference can target a domain type without requiring that resource to be created by the current compilation. Extension authors can work against public contracts without depending on the private engine implementation.

## My contributions

My work centers on the architecture, public contract boundaries, and the documentation that explains them. The repository records include contract implementation and review, release documentation, and the separation of project-wide decisions from repository-specific implementation notes.

## Current status

As reviewed on October 23, 2026, the public documentation describes active architectural design and foundation work. Proposed ADRs and the complete compiler pipeline should not be read as a finished product. Public contracts have an implementation and tests; the engine remains private.

The distinction between a proposed architecture and implementation evidence is part of the project itself. The public documentation is the place to follow that evolution.

## Related writing

[Four Principles for AI-Assisted Development]({% post_url 2026-10-08-four-principles %}) explains how I began carrying engineering intent into my independent projects. [Documentation Preserves It]({% post_url 2026-10-15-documentation-preserves-it %}) discusses why recording the reasoning matters as much as recording the result.
