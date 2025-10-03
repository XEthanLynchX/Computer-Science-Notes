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

  

pool:

  vmImage: ubuntu-latest

  

steps:

- task: NodeTool@0

  inputs:

    versionSpec: '20.x'

  displayName: 'Install Node.js'

  

- script: |

    npm install

    npm run build

  displayName: 'npm install and build'

- task: CopyFiles@2

  inputs:

    contents: 'dist/**'

    targetFolder: $(Build.ArtifactStagingDirectory)

- task: CopyFiles@2

  inputs:

    contents: 'package.json'

    targetFolder: $(Build.ArtifactStagingDirectory)

    task: CopyFiles@2

- task: CopyFiles@2

  inputs:

    contents: '.npmrc'

    targetFolder: $(Build.ArtifactStagingDirectory)

- task: CopyFiles@2

  inputs:

    contents: 'README.md'

    targetFolder: $(Build.ArtifactStagingDirectory)

- task: PublishBuildArtifacts@1

  inputs:

    PathtoPublish: '$(Build.ArtifactStagingDirectory)'

    ArtifactName: 'graph-client'

    publishLocation: 'Container'
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
- Pair this with an .artifactignore to exclude files from the build folder (Ex: src folder and only package production dependencies)
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
---


## Post-Deployment Checklist

- Confirm build succeeded within DevOps and github
- Confirm the artifact is built correctly in DevOps ( No src code )