# Projects portfolio implementation

Review date: October 23, 2026. This document is excluded from Jekyll output.

## Discovery and decisions

The repository uses Jekyll, `mmistakes/minimal-mistakes@4.28.0`, and its dark skin. Navigation is in `_data/navigation.yml`; `_pages` holds archives and series; posts use the theme's `single` layout. There was no project collection, local layout, Gemfile, test runner, checked-in deployment workflow, AGENTS.md, or established decision-record format. The working tree was initially clean.

The existing public site, About page, and Engineering Intent articles informed the structure and tone. Hosting settings and CNAME are unchanged. No push or deployment is part of this work.

- `_projects` owns each project's metadata and narrative, generating `/projects/:name/`.
- `_layouts/project.html` inherits the theme's `single` layout. The theme still owns navigation, typography, titles, canonical links, and footer.
- The landing page uses one card include and collection metadata. The three lead projects receive full-width cards, the other featured projects paired cards, and earlier projects smaller cards. All stack on narrow screens.
- The scoped stylesheet loads through the theme's actual `head/custom.html` hook. The old `_includes/custom-head.html` is not the current theme hook and is left untouched.
- Navigation, post defaults, articles, series taxonomy, and blog permalinks remain unchanged.
- The theme's 4.28.0 pagination include is retained verbatim inside a collection guard: projects use their explicit back link instead of alphabetical previous/next links. Post pagination behavior is unchanged; review this override on theme upgrades.
- The Gemfile and HTML check support local verification. No browser-side dependency or deployment workflow was added.

## URL preservation

| Existing URL | Result |
| --- | --- |
| `/projects/` | Rebuilt landing page |
| `/projects/azdevops` | Redirect to `/projects/azdevops/` |
| `/projects/azdevops/` | New overview; original module reference retained at `/projects/azdevops/reference/` |
| `/projects/poshmongo` | Redirect to `/projects/poshmongo/` rather than the landing page |
| `/projects/modposh/logger` | Original reference retained, with link to new overview |
| Other historical project pages | Unchanged; no longer emphasized on landing page |

## Project inventory and evidence

GitHub metadata verified visibility and archive flags. Local README files, source structure, selected source/project files, and commit history were inspected where available. Current public documentation takes precedence over stale local copies.

| Project | Verified public repositories | Evidence and boundary |
| --- | --- | --- |
| BAT | None; employer-owned | User brief and existing About page. Team contribution language; no internal implementation copied. |
| llamarc42 | [docs](https://github.com/llamarc42/docs), [core](https://github.com/llamarc42/core), [cli](https://github.com/llamarc42/cli) | Current project definition, v0.1 contracts, Core validation source and CLI README. Core implemented; CLI planned. Legacy local-copilot direction is historical. |
| InfrastructureIntent | [documentation](https://github.com/InfrastructureIntent/documentation), [abstractions](https://github.com/InfrastructureIntent/abstractions) | Public architecture, contract README and changelog. Architecture exploration distinguished from implementation; private engine details omitted. |
| XML2Doc | [xml2doc](https://github.com/mod-posh/xml2doc) | Public README describes 2.4.0; local README still says 2.1.0. Public feature description used. Local renderer/project structure and CLI-validation history inspected. |
| YAML2Doc | [yaml2doc](https://github.com/mod-posh/yaml2doc) | README, engine, dialects, renderer, CLI structure, history. No semantic-validation claim. |
| ADO CLI | [ado](https://github.com/jeffpatton1971/ado) | Public README, local design/capability documentation and command/client structure. Candidate is not described as a published release. |
| Jira CLI | [jira](https://github.com/jeffpatton1971/jira) | Public README and architecture, local commands and credential adapter structure. Jira Cloud and source installation scope. |
| ModelMetrics | No public repository | Private visibility verified. Public page uses the project purpose supplied by the user. |
| PoshMongo | [PoshMongo](https://github.com/PoshMongo/PoshMongo) | README, cmdlet source and project file. C# cmdlets using MongoDB.Driver. |
| AzDevOps | [AzDevOps](https://github.com/Azure-Devops-PowerShell-Module/AzDevOps) | Public README, retained site reference, local history. Planned tests/pipelines not presented as delivered. |
| ModPosh.Logger | [Logger](https://github.com/mod-posh/Logger) | README, logger implementation, factory/configuration/interface structure, history. C# library usable from PowerShell. |

All linked repositories were public and not archived at review time. That does not establish an active maintenance schedule. No project is labeled abandoned, deprecated, or unmaintained.

GitHub blob SHAs for key reviewed sources: InfrastructureIntent documentation README `24876e930e5d2a8fcc2ca5ee6e7c468c317819e9`; architecture `cd2074db8f66ef90b83baebe07f624a64a2289d0`; llamarc42 project definition `1158c260b1f871826056bf0b93a928c2a8e89cc7`; Core README `fbd91181cf834df1231fda4aff93f34c26ab7dc8`; XML2Doc README `87b3969dd69a3a2ed3e8d9a31c3519f4b3aa0a0c`; ADO README `d615b660af3b032df1851fec53f71ba31f7ceaba`; Jira README `8adf428c4a800f9a9e7b91c1a8b23f9ff26ed38d`.

## Confidentiality and editorial follow-ups

- The local `global-build` workspace exists. Only its directory inventory was checked. No BAT source, internal diagrams, customer data, endpoints, or documents were used in public copy. The case study uses the user brief and existing public-site writing.
- The private InfrastructureIntent engine README was consulted for orientation. Public technical claims rely on public architecture/contracts. No private engine URL or implementation excerpt appears on the site.
- ModelMetrics' local README was inspected before visibility was confirmed. Implementation details, workloads, machine profiles, raw evidence, configurations, and the private repository URL are excluded. A fuller page requires a deliberately public description or documentation.
- Contribution wording uses supplied history, articles, and attributable commits. Commit attribution does not establish sole authorship or exclude AI assistance. No sole-ownership claim is made for team work.
- A later editorial pass could add a concrete approved example and more precise personal design responsibility for each smaller tool. Current contribution sections stay within the available evidence.
- Review the conservative BAT case study against any employer disclosure constraints beyond those in the task before publication. Additional private detail is not required.
- Recheck dated release and maintenance statements before publishing. No repository visibility change is authorized or performed.

## Validation

```text
bundle install
bundle exec jekyll build --strict_front_matter
bundle exec ruby scripts/check-portfolio.rb
bundle exec jekyll serve --host 127.0.0.1
```

The HTML check covers 8 featured / 3 earlier projects, order, metadata, canonicals, primary headings, duplicate IDs, local links and fragments, rendered Liquid, stylesheet loading, and retained reference/series pages. For a subpath build, supply the output directory and baseurl as its two arguments.

### Results

- Full Jekyll 3.10.0 build with strict front matter passed. The final rebuild completed in about 60 seconds.
- All 11 project pages and the 8/3 landing-page split passed generated-HTML checks, including internal links and article anchors. Canonical URLs are correct and no unresolved Liquid or duplicate IDs were found.
- The build rendered 214 existing posts (211 under `/blog/` and 3 under `/2026/`). The remaining source post is explicitly unpublished. Engineering Intent, About, category/tag archives, and historical project pages remain in the output. Source comparison confirms no changes to posts, series/pages, navigation, About, or CNAME.
- The old extensionless AzDevOps and PoshMongo routes render redirect files pointing to their new trailing-slash URLs. The retained Logger reference remains available.
- Desktop viewport review at 1440px confirmed two-column featured tooling and three-column earlier-project grids. At 390px all cards stack, with document width equal to scroll width (no horizontal overflow). The landing page, earlier-project anchor, BAT and PoshMongo detail pages, and return navigation were inspected through the browser.
- No private BAT source, internal documents, endpoints, diagrams, or customer examples appear in the added site content. ModelMetrics has no private repository link or implementation details.
- `git diff --check` passed. Git reports normal LF-to-CRLF normalization notices on this Windows checkout.

### Local build environment qualification

Ruby was not installed, Docker's engine was unavailable, and portable MRI Ruby could not compile its native websocket dependency without MSYS2. Verification therefore used an isolated JRuby runtime and locally cached dependencies. JRuby's network layer could not establish a loopback connection, so the exact Minimal Mistakes 4.28.0 archive was fetched separately and staged with the entire site. A temporary configuration set `remote_theme: false` for that offline build. The tracked remote-theme configuration is unchanged.

This was an actual Jekyll render, including Sass, Liquid, collection routing, feed, sitemap, and redirects; it was not a mock HTML preview. The hosting provider's deployment and remote-theme download path were not exercised. Java deprecation and optional `fiddle` warnings did not prevent the build. Temporary runtime, staging, and preview files live in ignored `.jekyll-tools/`; gem caches live in ignored `vendor/`.

The Gemfile follows the [GitHub Pages Jekyll/plugin line](https://pages.github.com/versions/). Its lockfile includes Windows, Java, and Linux platforms. The parser pin provides a Java-compatible package; base64 and bigdecimal support newer Ruby runtimes. Standard Ruby with Devkit remains the simplest local setup.

## File inventory

Created:

- `_projects/{bat,infrastructureintent,llamarc42,xml2doc,yaml2doc,ado-cli,jira-cli,modelmetrics,poshmongo,azdevops,modposh-logger}.md`
- `_layouts/project.html`
- `_includes/project-card.html`, `_includes/head/custom.html`, `_includes/post_pagination.html`
- `assets/css/projects.css`
- `projects/azdevops/legacy.md`
- `Gemfile`, `Gemfile.lock`, `README.md`
- `scripts/check-portfolio.rb`, `docs/projects-portfolio.md`

Modified: `_config.yml`, `.gitignore`, `projects/index.md`, `projects/azdevops/index.md`, `projects/poshmongo/index.md`, `projects/modposh/logger.md`.

Removed: none. No external repository was modified; nothing was committed, pushed, or deployed.
