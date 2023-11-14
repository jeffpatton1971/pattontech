---
layout: post
title:  "Azure Devops Pipelines : Lesson's Learned"
date:   2023-11-14 07:46:00 -0600
categories: blog
tags: November 2023 Development AzureDevops
author: Jeff
comments: true
published: true
---

Azure DevOps Pipelines serve as a cornerstone for automating software delivery, offering a robust framework to streamline CI/CD processes. In this exploration, we'll dissect four example pipelines, each highlighting a distinct aspect - 'stageDependencies,' 'dependencies,' variables, and parameters.

## Understanding 'stageDependencies' and 'dependencies'

In the world of Azure DevOps Pipelines, both 'stageDependencies' and 'dependencies' play a pivotal role in orchestrating the flow of your pipeline. Let's distinguish between them and understand when to use each.

## Unraveling 'stageDependencies'

In the first example, the utilization of 'stageDependencies' orchestrates a sequential order between the 'Build' and 'Deploy' stages. The 'Build' stage, encapsulating the application's compilation, acts as a prerequisite for the 'Deploy' stage. This dependency ensures that deployment only proceeds when the build is successfully completed, providing a structured and controlled progression through the pipeline.

```yaml
stages:
- stage: Build
  jobs:
  - job: BuildJob
    pool:
      vmImage: 'windows-latest'
    steps:
    - script: echo "Building the application"

- stage: Deploy
  dependsOn: Build
  jobs:
  - job: DeployJob
    pool:
      vmImage: 'windows-latest'
    steps:
    - script: echo "Deploying the application"
```

## Navigating 'dependencies'

The second example showcases the role of 'dependencies' within a job. Here, 'SecondTask' depends on the successful completion of 'FirstTask' within the same job. This setup ensures a sequential execution of tasks, where the output of one task serves as the input for another. 'dependencies' within a job prove instrumental in maintaining task order and inter-task dependencies. The 'name' property of tasks is instrumental in defining dependencies between tasks within a job or template. To ensure a smooth dependency chain, it is vital to place the 'name' property strategically.


```yaml
jobs:
- job: ExampleJob
  pool:
    vmImage: 'windows-latest'
  steps:
  - task: Task1
    name: FirstTask
    script: echo "Executing the first task"
  
  - task: Task2
    name: SecondTask
    script: echo "Executing the second task"
    dependsOn: FirstTask
```

## Proper Handling of Variables and Parameters

In addition to managing dependencies, understanding how to handle variables and parameters is crucial for building flexible and reusable pipelines.

## Mastering Variables in Pipelines

Moving beyond dependencies, our third example introduces the strategic use of variables at the pipeline level. The 'solution' variable holds the name of the solution file and is accessed in the 'BuildJob.' This centralized definition of common values simplifies maintenance and ensures consistency across the pipeline, showcasing the power of variables in maintaining a coherent and adaptable pipeline structure.

```yaml
variables:
  solution: 'MySolution.sln'

stages:
- stage: Build
  jobs:
  - job: BuildJob
    pool:
      vmImage: 'windows-latest'
    steps:
    - script: msbuild ${{ variables.solution }}
```

## Harnessing Parameters for Customization

Parameters allow you to make your pipeline more dynamic by accepting values from external sources. They are defined at the job or template level using the 'parameters' keyword. Our final example introduces the concept of parameters at the job level. The 'ExampleJob' accepts a parameter named 'inputParameter' with a default value. Parameters provide flexibility, allowing external inputs to customize job behavior. This adaptability is crucial for handling diverse scenarios, making pipelines more dynamic and responsive to changing requirements.

```yaml
jobs:
- job: ExampleJob
  pool:
    vmImage: 'windows-latest'
  parameters:
    - name: inputParameter
      default: 'defaultValue'
  steps:
  - script: echo "Using parameter: ${{ parameters.inputParameter }}"
```

By mastering the use of 'stageDependencies' and 'dependencies' for managing dependencies and understanding the proper handling of variables and parameters, you can create efficient, flexible, and reusable Azure DevOps Pipelines. The examples provided should serve as a guide to help you navigate these concepts seamlessly in your CI/CD processes. Happy coding!
