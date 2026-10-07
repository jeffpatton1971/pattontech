---
layout: single
title: Jeffrey Patton
permalink: /about/
---

# Jeffrey Patton

**Software, Platform & Cloud Engineer | Automation | C# / .NET | PowerShell | Azure | Infrastructure as Code | AI-Assisted Engineering**

I have spent more than three decades working with technology, beginning as a technical instructor and moving through enterprise systems administration, cloud engineering, DevOps, software development, and platform engineering.

The common thread through most of that career has been automation.

I tend to gravitate toward problems where people are doing something repeatedly, where the process varies depending on who performs it, or where a complicated technical workflow has grown difficult to understand and maintain. My first instinct is usually to figure out why the process works the way it does, define what should be consistent, and then build something that makes the repeatable parts automatic.

Over the years that has meant everything from login scripts and Active Directory automation to infrastructure-as-code, APIs, build pipelines, cloud provisioning systems, extensible .NET platforms, and more recently AI-assisted engineering.

I have worked from several different sides of IT: teaching people how technology works, operating production systems, supporting customers, designing infrastructure, writing automation, and building software used by other engineers. That combination has had a significant influence on how I approach engineering. I care about the code, but I also care about whether another person can understand it, operate it, troubleshoot it, and extend it later.

## Rackspace Technology

I spent just over ten years at Rackspace, joining the Microsoft Azure organization in 2016 and eventually moving from customer-facing cloud engineering into build automation, software development, and platform engineering.

The progression was fairly natural. I started by supporting cloud environments, moved into building them, began automating those builds, and ultimately spent much of my time building systems intended to automate the automation itself.

### Build Automation / Platform Engineering

My most recent work focused on the Build Automation Tool, or BAT: an internal platform developed to make infrastructure builds consistent, repeatable, testable, and easier for engineering teams to maintain.

The problem BAT addressed was larger than simply generating Terraform. Cloud products had their own resources, conventions, templates, defaults, and deployment requirements. Input could originate from different systems, and the same design needed to reliably produce the same approved result regardless of the engineer executing the build.

Over several generations, BAT evolved into a modular .NET platform that separated the core processing engine from the cloud-specific resources and deployment technologies surrounding it.

My work included architecture, C#/.NET development, REST APIs, PowerShell tooling, provider models, Terraform generation, validation, defaults, diagnostics, testing, CI/CD, package management, documentation, release processes, and integration across multiple repositories and engineering teams.

The later architecture used independently versioned components and extension points for concerns such as input processing, resource definitions, provider-specific behavior, projections, and rendered output. This allowed the core platform to remain relatively agnostic while individual cloud products supplied the knowledge required to understand and generate their infrastructure.

Some of the engineering concerns that became particularly important included:

- Deterministic generation of deployment artifacts
- Strong validation and useful diagnostics
- Clear public extension contracts
- Provider and renderer extensibility
- Separation between cloud resource models and Terraform representations
- Independently versioned NuGet packages
- Automated regression and golden-file testing
- API-driven execution
- CI/CD across multiple repositories
- Architecture Decision Records and governed technical documentation
- Backward compatibility and controlled evolution of shared contracts

The platform was developed primarily in **C# and .NET**, with extensive use of **PowerShell, Terraform, Azure DevOps, GitHub, REST APIs, YAML, JSON**, and cloud-specific infrastructure technologies.

What interested me most about BAT was not simply generating infrastructure. It was finding the boundaries that allowed a complicated system to remain understandable as additional products, providers, resources, and deployment targets were added.

### AI and Engineering

I have also spent a significant amount of time exploring where generative AI can actually improve engineering workflows rather than simply adding AI to an existing process.

One early proof of concept used Azure OpenAI with Rackspace's existing infrastructure templates as source material. The question was whether an engineer could describe infrastructure using natural language and reliably receive the same approved Terraform that Rackspace already used.

That distinction was important. Generating *valid-looking* Terraform was not enough. Infrastructure automation needs predictable results.

Initial responses varied between requests, much like the results I was seeing from general-purpose coding assistants. By adjusting model parameters, including temperature, and constraining the model with known-good infrastructure definitions, I was able to make the results considerably more consistent.

The experiment reinforced something that continues to shape how I think about AI engineering: probabilistic systems need explicit boundaries, validation, and deterministic components around them when they are being used for infrastructure or other operational workloads.

Today I use AI extensively as an engineering tool while continuing to evaluate the strengths and limitations of different approaches. My interests include OpenAI, Anthropic, Microsoft's AI ecosystem, local models through tools such as Ollama, open-weight models, coding agents, retrieval and context management, and the effect these systems are having on software engineering.

I am particularly interested in the boundary between traditional deterministic software and AI-driven systems: figuring out which portions of a problem genuinely benefit from a language model and which should remain conventional code.

### Azure Build Engineering

Before working on the broader BAT platform, I was part of Rackspace's Azure Build Team and eventually served as a lead engineer.

The team took infrastructure designs and turned them into working Azure environments. Much of my focus became making that process faster, more consistent, and less dependent on an individual engineer knowing every step from memory.

One of the tools I developed was a build schema that could be used from Visual Studio or Visual Studio Code. An engineer could create a JSON build document and tab through the schema-defined fields to describe the environment being built.

I then developed a separate PowerShell module that consumed that build document and generated the templates and supporting documentation required for the deployment.

That combination moved knowledge out of an individual engineer's head and into a repeatable process. It also meant that validation and standardization could begin before deployment rather than after something had already been built incorrectly.

Other work included maintaining and standardizing ARM templates, moving infrastructure code into managed source-control workflows, creating automated deployment pipelines, developing PowerShell tooling, and working with architects and engineers to translate designs into deployable Azure infrastructure.

Automation reduced portions of the Azure build process from work measured in days to work that could be completed in hours.

Much of what eventually became BAT grew from the same question I had been asking throughout the Build Team: once we have automated an individual task, how do we automate the process around it so we do not have to solve the same problem again?

### Azure Support

I originally joined Rackspace working directly with customers running workloads in Microsoft Azure.

The work ranged from operating-system issues to troubleshooting cloud environments involving networking, VPN connectivity, load balancers, Application Gateways, backups, automation, and cost-management concerns.

That experience remains valuable to the way I design software today. Infrastructure tools ultimately interact with real environments operated by real people, and troubleshooting production systems gives you a different perspective on error handling, observability, defaults, documentation, and diagnostics than development alone.

## Lowe's — Iris Home Automation

Before Rackspace, I worked as a Senior Systems Administrator supporting the infrastructure behind Lowe's Iris home-automation platform.

I joined during an important transition in Microsoft Azure. Much of the existing environment still used Azure Service Management, or ASM, while Azure Resource Manager and ARM templates had only recently become available.

A large portion of my work involved developing ARM templates and helping move infrastructure provisioning away from manual operations and older deployment methods toward repeatable infrastructure-as-code.

We later automated those templates to provision servers into the Iris environment.

At its peak, the platform operated more than **4,000 Azure virtual machines** supporting hundreds of thousands of customers and connected devices. Working at that scale made consistency and repeatability necessities rather than conveniences.

Our small team was responsible for both operating the environment and improving the way it was provisioned and maintained. My work included Azure infrastructure, ARM templates, configuration management with Salt, PowerShell automation, and virtual-machine provisioning and maintenance.

Although my time at Lowe's was relatively short, it was an important transition in my career. Infrastructure-as-code stopped being something I was experimenting with and became part of how we operated a large production cloud environment.

## University of Kansas

I spent several years at the University of Kansas working both within the School of Engineering and later in KU IT, the university's central IT organization.

In many ways, this is where the approach I still use today became foundational.

Automation was not a separate discipline or a special project. It became the normal way I approached recurring work.

### School of Engineering

As Assistant Director of IT for the University of Kansas School of Engineering, I helped support an environment with more than **2,000 desktop systems** spread across engineering departments, classrooms, computer labs, faculty offices, and specialized facilities.

Those machines could not all be treated alike. Different engineering disciplines required different software, configurations, licensing, peripherals, and lab environments.

We used **System Center Configuration Manager** extensively for automated operating-system and software deployment, allowing us to maintain standardized systems while still supporting the very different requirements of individual engineering labs.

Server monitoring was handled through **System Center Operations Manager**, and I developed management packs for situations where the standard monitoring capabilities did not address what we needed.

One of the more interesting projects was a unified login system.

The environment needed to understand both **who the user was** and **where they were logging in**.

A faculty member or staff member could move between offices or systems within the School of Engineering and retain access to their own desktop environment and files. At the same time, their session needed to adapt to the location they were currently using so that resources such as nearby printers were automatically available.

The login process evaluated the user, machine, and location and assembled the appropriate environment dynamically.

It was an early example of a pattern that has repeated throughout my career: gather a relatively small amount of structured information, apply consistent rules to it, and automate the complicated work behind the scenes.

The School of Engineering also relied heavily on Active Directory, Windows Server, VMware, storage systems, software deployment, monitoring, and scripting. During this period I increasingly moved administrative automation away from VBScript and toward PowerShell.

### KU IT / Central IT

I later moved into **KU IT**, the university's central IT organization, where the scale of the systems and automation changed considerably.

Among my responsibilities was managing System Center Operations Manager monitoring for the university's fleet of roughly **20 Active Directory domain controllers across three campuses**.

Automation remained central to nearly every aspect of the job.

One of the largest automated workflows used **System Center Orchestrator**, often called SCOrch, to manage identity provisioning for the roughly **10,000 new students entering the university each year**.

The workflow handled more than creation of the initial account. It coordinated the surrounding provisioning required to turn that identity into a usable university account, including creation and enablement of the user's Lync address and related services.

The same automation framework was then used to onboard and enable faculty and staff across the university.

At that scale, automation was about much more than saving administrator time. A repeatable workflow meant thousands of people could be provisioned consistently, required services could be coordinated across systems, and the process could be monitored and maintained without requiring an administrator to manually reproduce the same sequence of actions thousands of times.

We also automated other account-management processes, system monitoring, remediation, administrative workflows, and recurring infrastructure operations. I worked extensively with **Active Directory, PowerShell, System Center Configuration Manager, System Center Operations Manager, System Center Orchestrator, Windows Server, VMware**, and related Microsoft enterprise technologies.

One assignment began with a deceptively simple request:

**Combine PowerShell and VMware.**

What that eventually became was an ASP.NET application that allowed a user to provision a VMware virtual machine on the ESXi cluster without manually stepping through the underlying infrastructure process.

The application handled the surrounding work as well. Once the server was provisioned, it could be incorporated into the appropriate monitoring and inventory systems automatically.

That project was also my first significant exposure to developing against APIs.

Looking back, it is a good example of how projects tend to grow for me. A request begins as automation for one technical operation. Then I start asking what information the automation needs, how that information should be represented, how somebody else should interact with it, what systems should be updated afterward, and which pieces should become reusable.

Eventually the script becomes a tool, the tool becomes an application, and the application starts becoming a platform.

## Technical Training

Before moving full-time into enterprise IT, I spent more than a decade teaching technology.

At Bryan College and Americomp, I taught Microsoft technologies and certification courses while developing curriculum, labs, exercises, study material, and assessments.

Teaching had a lasting impact on how I work as an engineer.

Explaining a technical concept to someone else quickly exposes whether you actually understand it. It also teaches you that people approach problems differently. Documentation, naming, examples, error messages, and interfaces all become easier to evaluate when you have spent years watching people encounter unfamiliar technology for the first time.

That experience still influences the way I write documentation, review designs, mentor engineers, and approach technical discussions.

I believe people should be able to ask questions without worrying about whether the question is sufficiently sophisticated. Understanding the question is usually more useful than demonstrating that you already know the answer.

## How I Work

I enjoy engineering problems that sit at the intersection of software and infrastructure.

I am comfortable starting with an operational process, understanding how it actually works, and then progressively turning it into software. Sometimes that means a PowerShell script. Then it becomes a module. Then somebody needs to call it remotely, so it becomes an API. Then more than one system needs to use it, and suddenly I am designing an extensible .NET architecture with independently versioned components.

That progression is not particularly hypothetical. It is more or less how a lot of my work has happened.

Two simple ideas I picked up early in my career still sit somewhere in the back of my mind when I design systems:

**KISS — Keep It Simple, Stupid.**

And the **six Ps — Proper Prior Planning Prevents Piss-Poor Performance.**

They may sound informal, but both point toward something I take seriously.

I am willing to put significant effort into understanding a problem before committing to an implementation. If the important boundaries, assumptions, contracts, and likely areas of change can be identified early, the resulting system can remain simpler and more flexible for much longer.

For me, making something flexible or technology-agnostic does not mean trying to predict every future requirement. It means avoiding unnecessary coupling and making deliberate decisions about which parts of a system should know about each other.

That upfront work tends to make everything afterward easier: adding another implementation, changing a backend, writing tests, diagnosing failures, or handing the system to another engineer.

I prefer systems that make their behavior explicit and testable. I value deterministic output, strong contracts, useful diagnostics, automated testing, small composable components, and documentation that explains **why** a system was designed the way it was rather than merely describing what the code does.

I also work iteratively. A small proof of concept that answers the most important architectural question is usually more valuable than attempting to design an entire finished system based solely on assumptions.

And despite spending much of my career automating things, I don't believe automation means removing people from the process.

Good automation removes repetition, unnecessary variation, and opportunities for avoidable mistakes so that engineers can spend more time on the parts of a problem that actually require engineering judgment.

## Technologies

My career spans several generations of enterprise, cloud, and software technology. Some technologies have remained useful for decades; others have come and gone while the engineering principles behind them have remained surprisingly consistent.

### Software & Development

C# · .NET · ASP.NET · REST APIs · PowerShell · Git · GitHub · Visual Studio · Visual Studio Code

### Cloud & Infrastructure

Microsoft Azure · Terraform · Infrastructure as Code · ARM · Azure Service Management · VMware · ESXi · Windows Server · Linux

### Platform & DevOps Engineering

Azure DevOps · GitHub Actions · CI/CD · NuGet · Package Versioning · API Integration · Automated Testing · Configuration Management

### Architecture

Extensible Platforms · Provider Models · Plugin Architectures · Domain Modeling · Validation · Deterministic Generation · Infrastructure Automation · API Design

### AI & Developer Tooling

OpenAI · Azure OpenAI · Anthropic · GitHub Copilot · Codex · Ollama · Local and Open-Weight Models · Retrieval-Augmented Workflows · AI-Assisted Software Development

### Enterprise Systems

Active Directory · SCCM · SCOM · System Center Orchestrator · VMware/ESXi · Windows Deployment · Enterprise Monitoring

## Education

**Washburn University**  
Computer Science

## Certifications

My certification history spans several generations of Microsoft technology, including Azure, Windows Server, systems administration, systems engineering, and technical instruction.

Some of the certifications I earned no longer exist.

I consider that less a problem than a fairly accurate indication of how long I have been doing this.

The individual products, certification tracks, and acronyms change. The underlying work of understanding systems, troubleshooting them, automating them, and building better ways to operate them has remained much more consistent.

## Beyond the Résumé

This site has existed in one form or another for many years, and much of what is here reflects the problems I happened to be working on at the time.

Older posts cover Windows administration, Active Directory, System Center, and PowerShell. Later work moves toward Azure, infrastructure-as-code, CI/CD, APIs, and multi-cloud automation. More recent work increasingly involves C#, .NET, platform architecture, AI-assisted development, local models, and experimenting with the rapidly changing ecosystem around large language models.

That progression is a fairly accurate representation of my career.

The individual technologies change.

The part I continue to enjoy is figuring out how something works, finding the repetitive or unnecessarily complicated parts, and building a better way to do it.
