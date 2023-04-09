# Azure DevOps Project 1

# Creating a CI/CD pipeline for a ASP dot net web application using Azure DevOps

# Step 1: Creating a Sample .NET Application Code using Visual Studio

1. Open Visual Studio and click on "Create a new project".
2. Select "ASP.NET Core Web Application" and click on "Next".
3. Choose a name and location for your project and click on "Create".
4. In the next window, select "ASP .NET Core Web Application (Model-View-Controller)" and click on "Create".
5. Wait for Visual Studio to create the project and open the solution.
6. In the Solution Explorer, open the "Pages" and go to "Views > Home" folder and double-click on "Index.cshtml".
7. Modify the contents of the file as per your requirements.
8. Save the file.
9. Build and Run the project.

That's it! Your sample .NET code is now ready. (This sample dot net code can be found here: https://github.com/01abhaysharma/sample_dotnetapp_ADOproject1)

Watch this hands-on video here: https://youtu.be/5tgbfaZr1UA

# Step 2: Uploading Sample dot net Code to GitHub

1. Go to the GitHub website and sign in to your account.
2. On the homepage, click on the "Create a new repository" button or go to your profile and click on the "New" button.
3. Give your repository a name and add a description (optional).
4. Choose whether you want your repository to be public or private.
5. Initialize the repository with a README file.
6. Click on the "Create repository" button.
7. On the next screen, you can upload your existing code to the repository. You can drag and drop the files or click on the "Upload files" button.
8. Once your files are uploaded, you can commit them to the repository by adding a commit message and clicking on the "Commit changes" button.

That's it! Your code is now uploaded to your GitHub repository.

# Step 3: Creating a Resource Group and App Service under that Resource Group in Azure Portal

1. Login to your Azure portal and select "Resource groups" from the left-hand menu.
2. Click on the "+ Add" button to create a new resource group.
3. Provide a name for the resource group and select a region. Click on the "Review + create" button.
4. Review the settings and click on the "Create" button.
5. Once the resource group is created, go to the "App Services" section in the Azure portal.
6. Click on the "+ Add" button to create a new app service.
7. Select the appropriate subscription, resource group, and name for the app service.
8. Select a runtime stack and operating system for your app service. For example, if you used ASP.NET Core, you can select ".NET Core" as the runtime stack and "Windows" as the operating system.
9. Choose an appropriate region for the app service and click on the "Review + create" button.
10. Review the settings and click on the "Create" button.
11. Wait for a few minutes until the app service is created.

That's it! Your App service is now created.

# Step 4: Creating a New Azure DevOps Project and Repository

1. Sign in to your Azure DevOps account (https://dev.azure.com/) or create a new account if you don't have one.
2. Click on "Create project" to create a new Azure DevOps project.
3. Choose an appropriate project name and description.
4. Select "Git" as the version control system for your project.
5. Click on "Create" to create the project.
6. Once the project is created, navigate to the "Repos" section and click on "Import".
7. Choose "Import a repository" and enter the URL of the .NET 6 web application codebase that you downloaded and modified in Step 2.
8. Click on "Import" to import the codebase into the Azure DevOps repository.
9. Once you have created the Azure DevOps project and imported the .NET 6 web application codebase, you can proceed to the next step, which is to create a new pipeline in Azure DevOps.

## Step 5: Creating a New Service Connection in Azure DevOps:

1. Navigate to your Azure DevOps project and click on "Project settings" in the bottom left corner.
2. In the Project Settings menu, click on "Service connections" under "Pipelines".
3. Click on the "+ New service connection" button in the top right corner and select the service you want to connect to. For this project, select "Azure Resource Manager" as the type of service connection.
4. Select the scope type as "Subscription". A new window will open for authentication to authenticate with Azure.
5. Once authenticated, select the resource group, provide a name for the service connection, and check "Grant access to all pipelines". Then click on "Save".
6. That's it! Your service connection is now created.

After creating the service connection, you can use it in your pipeline YAML file to deploy your application to the connected environment.


# Step 6: Creating a New Pipeline in Azure DevOps

1. Navigate to your Azure DevOps project and click on "Pipelines" in the left sidebar.
2. Click on "New pipeline" to create a new pipeline.
3. Choose the source of your code. Select "Azure Repos Git" and choose your repository and branch.
4. Choose a pipeline template to start with. Select the "Starter pipeline" template.
5. In the "Configure your pipeline" step, choose the build agent pool you want to use. The default agent pool should work fine for this project.
6. In the "Review your pipeline YAML" step, review the pipeline YAML file and make any necessary modifications. Below is an example YAML file for this project:

## YAML used for this project:

```yaml
trigger:
- main

pool:
  vmImage: 'windows-latest'

variables:
  buildConfiguration: 'Release'

steps:
- task: UseDotNet@2
  inputs:
    packageType: 'sdk'
    version: '6.0.x'
    includePreviewVersions: true

- task: DotNetCoreCLI@2
  inputs:
    command: 'build'
    projects: '**/*.csproj'
    arguments: '--configuration $(buildConfiguration)'

- task: DotNetCoreCLI@2
  inputs:
    command: 'test'
    projects: '**/*Tests.csproj'
    arguments: '--configuration $(buildConfiguration)'

- task: DotNetCoreCLI@2
  inputs:
    command: 'publish'
    projects: '**/*.csproj'
    arguments: '--configuration $(buildConfiguration) --output $(Build.ArtifactStagingDirectory)'

- task: PublishBuildArtifacts@1
  inputs:
    artifactName: 'package'
    pathToPublish: '$(Build.ArtifactStagingDirectory)'
    publishLocation: 'Container'

- task: DownloadBuildArtifacts@0
  inputs:
    artifactName: 'package'
    downloadPath: '$(System.ArtifactsDirectory)'

- task: AzureWebApp@1
  inputs:
    azureSubscription: 'Learning-SC'
    appName: 'Learningappservice'
    package: '$(System.ArtifactsDirectory)/package/**/*.zip'

```
7. Click on "Save and run" to save the pipeline and trigger the first build.


That's it! This project is completed now.
