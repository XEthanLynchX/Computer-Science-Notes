## Prerequisites

- Create a Pipeline folder 
- Build.yaml folder with: 
	- Trigger on main
	- pool 
	- steps to install packages, build , audit, etc 
	- format build output file

--- 

## Sample build.yaml

``` yaml
trigger:

- main

  

stages:

- stage: BuildAndPublish

  displayName: Build and Publish Artifact

  jobs:

  - job: BuildJob

    displayName: Build GraphService

    pool:

      vmImage: ubuntu-latest

    steps:

    - task: NodeTool@0

      displayName: Use Node.js 20.x

      inputs:

        versionSpec: '20.x'

  

    - task: npmAuthenticate@0

      displayName: Authenticate private npm registry

      inputs:

        workingFile: '.npmrc'

        customEndpoint: 'npm-ao'

  

    - script: |

        npm install

        npm run build

        npm prune --production

        npm run audit:high

      displayName: Install, build, prune, audit

  

    - task: PublishPipelineArtifact@1

      inputs:

        targetPath: '$(System.DefaultWorkingDirectory)'

        artifact: 'build'

        publishLocation: 'pipeline'

  

- stage: PublishDeploymentFolder

  displayName: Archive and Publish Zip

  dependsOn: BuildAndPublish

  jobs:

  - job: ArchiveJob

    displayName: Archive and Publish Zip

    pool:

      vmImage: ubuntu-latest

    steps:

    - checkout: none

    - download: current

    - task: ArchiveFiles@2

      displayName: 'Archive files'

      inputs:

        rootFolderOrFile: '$(Pipeline.Workspace)/build'

        includeRootFolder: false

        archiveType: zip

        archiveFile: $(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip

        replaceExistingArchive: true

    - upload: $(Build.ArtifactStagingDirectory)/$(Build.BuildId).zip

      artifact: drop
```

## Artifact ignore

```
# Exclude everything
**/*

# Add what we want back in
!node_modules/**
!dist/**
!package.json
!host.json
```
---

## Tasks and Their Functions
### 1. NodeTool@0
- Installs a specific version of Node.js
```yaml 
- task: NodeTool@0
  inputs:
    versionSpec: '20.x'
```

### 2. CopyFile@2 
- Copies files from one place to another (example repo to artifact) 
- Not necessary however if you're using Archive(For DevOps build folder)
``` yaml 
- task: CopyFiles@2
  inputs:
    contents: '**/*.js'
    targetFolder: '$(Build.ArtifactStagingDirectory)'
```

### 3. ArchiveFiles@2
- Creates a ZIP folder archive from a specified folder or set of files. Mainly used to package builds for deplyment and create downloadable artifacts for release pipelines
- Does not respect .artifactignore
``` yaml
- task: ArchiveFiles@2
  inputs:
    rootFolderOrFile: '$(Build.ArtifactStagingDirectory)'
    includeRootFolder: false
    archiveType: 'zip'
    archiveFile: '$(Build.ArtifactStagingDirectory)/output.zip'
    replaceExistingArchive: true
```

### 4. AzureFunctionApp@1 
- Connects Azure subscription using service connection string 
- Deploys ZIP package containing compiled app to specified Azure Function App
``` yaml 
- task: AzureFunctionApp@1
  inputs:
    azureSubscription: 'MyServiceConnection'
    appType: 'functionApp'
    appName: 'graphservice-dev'
    package: '$(Build.ArtifactStagingDirectory)/GraphService.zip'
```

### 4. PublishBuildArtifacts@1
- Publishes files to the **Pipeline** for later use (deployment)
``` yaml 
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(Build.ArtifactStagingDirectory)'
    ArtifactName: 'my-artifact'
```
## 5.  PublishPipelineArtifact@1 
- This respects the artifact ignore however it does not ZIP the file 

```yaml
- task: PublishPipelineArtifact@1
  inputs:
    targetPath: '$(System.DefaultWorkingDirectory)'
    artifact: 'build'
    publishLocation: 'pipeline'
```

---
## Post-Deployment Checklist

- Confirm build succeeded within DevOps and github
- Confirm the artifact is built correctly in DevOps ( No src code )