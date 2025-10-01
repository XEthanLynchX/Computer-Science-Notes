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

  vmImage: 'ubuntu-latest'

  

steps:

- script: |

    npm install

    npm run build

    npm prune --production

    npm run audit:high

    # Create a clean folder with only what we want

    mkdir package

    cp package.json package/

    cp host.json package/

    cp -R dist package/

    cp -R node_modules package/

  workingDirectory: '$(System.DefaultWorkingDirectory)'

  displayName: 'Build, prune and audit'

  

- task: ArchiveFiles@2

  displayName: "Archive files"

  inputs:

    rootFolderOrFile: "$(System.DefaultWorkingDirectory)/package"

    includeRootFolder: false

    archiveFile: "$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip"

- task: PublishBuildArtifacts@1

  inputs:

    PathtoPublish: '$(System.DefaultWorkingDirectory)/build$(Build.BuildId).zip'

    artifactName: 'drop'
```

---

## Post-Deployment Checklist

- Confirm build succeeded within DevOps and github
- Confirm the artifact is built correctly in DevOps ( No src code )