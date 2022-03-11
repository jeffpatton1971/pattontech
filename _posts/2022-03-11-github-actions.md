---
layout: post
title:  "Github Actions and PSDocs"
date:   2022-03-11 18:44:00 -0600
categories: blog
tags: March 2022 Review
author: Jeff
comments: true
published: true
---
Today I did some work on the [MultiCloudDeployment](https://github.com/MultiCloudDeployment) project. I had wanted to add the option to pass in [Resource Groups](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/manage-resource-groups-portal#what-is-a-resource-group) to the [ARM](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/overview) template,because that is something that can deployed against if you are deploying multiple templates at the same time. Additionally I was a little jealous of the [terraform-docs](https://terraform-docs.io/reference/terraform-docs/) utility, it generates some decent documentation from the Terraform. As I started my search I found the [PSDocs](https://github.com/microsoft/PSDocs) project as well as the [PSDocs.Azure](https://github.com/Azure/PSDocs.Azure) project that were exactly what I was looking for. These two modules work together to generate Markdown documentation files from discovered template files.

## PSDocs

This module extends PowerShell with domain specific language (DSL) keywords and cmdlets. You can generate documentation by using the DSL within a ['.doc.ps1'](https://github.com/microsoft/PSDocs#define-a-document) file. Using PowerShell you can design documentation using syntax that you are comfortable with and get documentation that anyone can easily consume.

## PSDocs.Azure

This module extends [PSDocs](https://github.com/microsoft/PSDocs) in such a way that instead of generate a document file, it processes an Arm Template. The only modifications that are required is leveraging the [metadata](https://docs.microsoft.com/en-us/dotnet/api/azure.resourcemanager.resources.models.parameterdefinitionsvaluemetadata?view=azure-dotnet-preview) object within your templates. Metadata can be added directly to the template, to the template parameters and to resources within the resource block of the template. The minimum property would be a description.

``` json
"metadata":{
    "description":"The resource name"
}
```

## Github 

The way I tend to manage my repo is to have a main branch that contains the working code, and then create a tagged release for each revision. For example I'll create a branch for adding documentation, when I've added all the metadata to the template I will merge that into main. Often there will be additional cleanup work that happens in main once that work is complete I will create a tag and we'll be done.

## Github Actions

So bringing all this together, I would like the documentation to be generated on a push into main. There is a Github Action for doing this, but I had some trouble with it so decided to start with the [Github Action PowerShell](https://github.com/Azure/PSDocs.Azure#using-with-github-actions) sample in the README. The example required some modification to work for me, I needed to get the ContentVersion of the template (for the deploy to azure link) and the name of the directory (the Github Repository name). Then I needed to update the README for the repo I found this article, [Using git commit in GitHub Actions](https://lannonbr.com/blog/2019-12-09-git-commit-in-actions).

### Arm Github Action

This assumes that I have a deployment template and a parameters template, so this wouldn't work with multiple template files in the same folder. Additionally this assumes we are working with templates in the root of the directory. Finally I set the name of the output file to README, as that's what shows up by default on the repo. The working file can be found [here](https://github.com/MultiCloudDeployment/arm-microsoftautomation-automationaccounts/edit/main/.github/workflows/publish-docs.yml).

``` yaml
name: Generate ARM templates docs
on:
  push:
    branches: [ main ]
jobs:
  arm_docs:
    name: Generate ARM template docs
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v2
    - name: Generate ARM markdowns
      run: |
        Install-Module -Name 'PSDocs.Azure' -Repository PSGallery -force;
        #$VerbosePreference = 'Continue';
        Get-AzDocTemplateFile | ForEach-Object {
          $TemplateFile = Get-Item -Path $_.TemplateFile;
          $RepoName = $TemplateFile.Directory.Parent.Name;
          $Template = Get-Content $TemplateFile |ConvertFrom-Json;
          $Version = $Template.ContentVersion;
          $docName = "README";
          $ReadmeHeader = "[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMultiCloudDeployment%2F$($RepoName)%2F$($Version)%2Fazuredeploy.json)"
          # Generate markdown
          Invoke-PSDocument -Module PSDocs.Azure -OutputPath "out/docs" -InputObject $TemplateFile.FullName -InstanceName $docName;
        };
        $Readme = Get-Content "out/docs/$($docName).md";
        Write-Output $ReadmeHeader |Out-File "out/docs/$($docName).md" -Force;
        Write-Output $Readme |Out-File "out/docs/$($docName).md" -Append;
        #Get-Content "out/docs/$($docName).md";
        Move-Item "out/docs/$($docName).md" . -Force
      shell: pwsh
    - name: Setup Git Config
      run:
        git config user.name "GitHub Action";
        git config user.email "<>";
    - name: Update ReadME
      run:
        git add *;
        git commit -m "Updating README with latest documentation";
        git push origin main
```

### Bicep Github Action

Currently [PSDocs.Azure](https://github.com/Azure/PSDocs.Azure/issues/106) does not support Bicep files natively, so originally I was thinking I would just copy/paste the README from the arm repo over the the bicep repo. There was a hint inside the issue that mentions it supports Bicep in ARM format. This means you could turn th bicep into an ARM template and then have PSDocs.Azure generate a documentation file. This requires a few things, first we will need to use Azure CLI and Bicep. Fortunately there is an [Azure CLI action](https://github.com/marketplace/actions/azure-cli-action) and Bicep includes a [build](https://docs.microsoft.com/en-us/azure/azure-resource-manager/bicep/bicep-cli#build) command that generates an ARM template from the Bicep. 

Once the template is generated the documentation is created using the same task from the ARM action, but as there is no Deploy to Azure feature for Bicep I don't need to generate that header. So the only extra thing that would be required is to delete the generate json file and simply commit back. Here is a link to the [Bicep Yaml](https://github.com/MultiCloudDeployment/bicep-microsoftautomation-automationaccounts/edit/main/.github/workflows/public-docs.yml).

``` yaml
name: Generate ARM templates docs
on:
  push:
    branches: [ main ]
jobs:
  arm_docs:
    name: Generate ARM template docs
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v2
    - name: Azure CLI
      uses: azure/CLI@v1
      with:
        azcliversion: 2.34.1
        inlinescript: |
          az bicep build --file azuredeploy.bicep
    - name: Generate ARM markdowns
      run: |
        Install-Module -Name 'PSDocs.Azure' -Repository PSGallery -force;
        #$VerbosePreference = 'Continue';
        Get-AzDocTemplateFile | ForEach-Object {
          $TemplateFile = Get-Item -Path $_.TemplateFile;
          $RepoName = $TemplateFile.Directory.Parent.Name;
          $Version = (Get-Content .\azuredeploy.bicep)[0].substring(3).split(':').trim()[1]
          $docName = "README";
          # Generate markdown
          Invoke-PSDocument -Module PSDocs.Azure -OutputPath "out/docs" -InputObject $TemplateFile.FullName -InstanceName $docName;
        };
        Move-Item "out/docs/$($docName).md" . -Force;
        Remove-Item .\azuredeploy.json -force;
      shell: pwsh
    - name: Setup Git Config
      run:
        git config user.name "GitHub Action";
        git config user.email "<>";
    - name: Update ReadME
      run:
        git add *;
        git commit -m "Updating README with latest documentation";
        git push origin main
```

## Configurations

The only other thing to be aware of is the configuration of PSDocs. You can do it in a few ways but the easiest for me is a ps-docs.yaml file. The only difference between the ARM version and the Bicep version is the snippets. For ARM I would like to see the parameters snippet, but for Bicep I don't need it as that's not a thing.

### Bicep Configuration
```yaml
configuration: {
  AZURE_USE_PARAMETER_FILE_SNIPPET: false,
  SAMPLE_USE_PARAMETERS_SNIPPET: true,
  AZURE_USE_COMMAND_LINE_SNIPPET: true
}

execution:
  languageMode: FullLanguage

markdown:
  encoding: Default
  skipEmptySections: true
  wrapSeparator: ' '
  columnPadding: MatchHeader
  useEdgePipes: WhenRequired

output:
  culture:
  - 'en-US'
  path: doc
```

### ARM Configuration
``` yaml
configuration: {
  AZURE_USE_PARAMETER_FILE_SNIPPET: true,
  SAMPLE_USE_PARAMETERS_SNIPPET: true,
  AZURE_USE_COMMAND_LINE_SNIPPET: true
}

execution:
  languageMode: FullLanguage

markdown:
  encoding: Default
  skipEmptySections: true
  wrapSeparator: ' '
  columnPadding: MatchHeader
  useEdgePipes: WhenRequired

output:
  culture:
  - 'en-US'
  path: docs
```

Now all I need to do is update the template repos with the actions so that each new repo I create will automatically include the proper actions to generate the documentation that I would like to see.
