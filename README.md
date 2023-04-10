# Azure DevOps Project 1

# Creating a CI/CD pipeline to deploy ASP .NET web application using Azure DevOps

# Step 1: Creating a sample .NET application code using Visual Studio

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

# Step 2: Uploading sample dot net code to GitHub

1. Go to the GitHub website and sign in to your account.
2. On the homepage, click on the "Create a new repository" button or go to your profile and click on the "New" button.
3. Give your repository a name and add a description (optional).
4. Choose whether you want your repository to be public or private.
5. Initialize the repository with a README file.
6. Click on the "Create repository" button.
7. On the next screen, you can upload your existing code to the repository. You can drag and drop the files or click on the "Upload files" button.
8. Once your files are uploaded, you can commit them to the repository by adding a commit message and clicking on the "Commit changes" button.

That's it! Your code is now uploaded to your GitHub repository.

Watch this hands-on video here: https://youtu.be/FieaJaYX6sY

# Step 3: Creating a Resource Group and App Service under that Resource Group in Azure Portal

1. Login to your Azure portal and select "Resource groups" from the left-hand menu.
2. Click on the "+ Add" button to create a new resource group.
3. Select Subscription, provide a name for the resource group and select a region. Click on the "Review + create" button.
4. Review the settings and click on the "Create" button.
5. Once the resource group is created, go to the "App Services" section in the Azure portal.
6. Click on the "+ Add" button to create a new app service.
7. Select the appropriate subscription, resource group, and name for the app service.
8. Select a publish, runtime stack and operating system for your app service. For example, if you used ASP.NET Core, you can select ".NET Core" as the runtime stack and "Windows" as the operating system and You can select publish as "code".
9. Choose an appropriate region for the app service, select the pricing plan, keep rest of the details as default and click on the "Review + create" button.
10. Review the settings and click on the "Create" button.
11. Wait for a few minutes until the app service is created.

That's it! Your Resource Group and App service under that Resource Group is now created.

Watch this hands-on video here: https://youtu.be/yyoO_usHZVM & https://youtu.be/F_IRVzsrsz0

# Step 4: Creating Organization and Project in Azure DevOps

1. Sign in to your Azure DevOps account (https://dev.azure.com/) or create a new account if you don't have one.
2. Click on the "New organization" button on the home page.
3. Enter a name for your organization. This should be a unique name that identifies your organization.
4. Choose the location where you want your organization to be created. This will determine the location of your organization's data centers and services.
5. Click on "Continue" to create your organization.
6. Wait for a few moments while Azure DevOps creates your new organization.
7. Once your organization is created, you can find it in the top left corner of the Azure DevOps page.
8. Click on "Create project" to create a new Azure DevOps project in the newly created organization.
9. Choose an appropriate project name and description.
10. Select "Git" as the version control system for your project.
11. Click on "Create" to create the project.

That's it! Your Organization and Project are now created in Azure DevOps

Watch this hands-on video here: https://youtu.be/KsSDcvJsU_4

# Step 5: Creating Repo in Azure DevOps project and Importing GitHub repository

1. In Azure DevOps, navigate to the project (Created in Step 4) where you want to create a new repo.
2. In the left-hand menu, click on "Repos".
3. In the repository page, click on the "Import" button located in center of page in "Import a repository" section.
4. In the import dialog box, select "Git" as the repository type and under clone URL paste the URL of GitHub repository that You want to import. (In this case Github repo will be sample dot net code repository that we created in Step 2).
5. You may need to authenticate to your GitHub account and authorize Azure DevOps to access your repositories.
6. Select the repository you want to import.
7. Click on "Import" to start the import process.
8. Wait for the import process to complete. Depending on the size of your codebase, this may take a few minutes or longer.

That's it! Your GitHub repository is now imported into Azure DevOps Project Repo, and you can start using it in your Azure DevOps pipelines.

Watch this hands-on video here: https://youtu.be/0QsuR-iA3BM

# Step 6: Creating a new Service Connection in Azure DevOps

1. Navigate to your Azure DevOps project and click on "Project settings" in the bottom left corner.
2. In the Project Settings menu, click on "Service connections" under "Pipelines".
3. Click on the "+ New service connection" button in the top right corner and select the service you want to connect to. For this project, select "Azure Resource Manager" as the type of service connection.
4. Select the scope type as "Subscription". A new window will open for authentication to authenticate with Azure.
5. Once authenticated, select the resource group, provide a name for the service connection, and check "Grant access to all pipelines". Then click on "Save".

That's it! Your service connection is now created. After creating the service connection, you can use it in your pipeline YAML file to deploy your application to the connected environment.

Watch this hands-on video here: https://youtu.be/dWPRq_8TAIE


# Step 7: Creating Azure DevOps Pipeline to Build and Deploy code to App service

1. Navigate to your Azure DevOps project and click on "Pipelines" in the left sidebar.
2. Click on "New pipeline" to create a new pipeline.
3. Choose the source of your code. Select "Azure Repos Git" and choose your repository and branch.
4. Choose a pipeline template to start with. Select the "Starter pipeline" template.
5. In the "Configure your pipeline" step, choose the build agent pool you want to use. The default agent pool should work fine for this project.
6. In the "Review your pipeline YAML" step, review the pipeline YAML file and make any necessary modifications. Below is an example YAML file for this project:

## YAML file used for this project:

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
    azureSubscription: '<service connection name>'
    appName: '<app service name>'
    package: '$(System.ArtifactsDirectory)/package/**/*.zip'

```
7. Click on "Save and run" to save the pipeline and trigger the pipeline.
8. Once pipeline is completed successfully, navigate to Your App service in Azure portal and click on default domain URL in overview section and see Your code is deployed to this App service.

## YAML file task details:

- UseDotNet task (`UseDotNet@2`) - This installs the .NET Core SDK and runtime needed to build and run the application (if it's not already installed).
- DotNetCoreCLI task (`DotNetCoreCLI@2`) (`build`) - This builds the application. It compiles the .NET Core application code into an executable form.
- DotNetCoreCLI task (`DotNetCoreCLI@2`) (`test`) - This task runs the unit tests in the .NET Core application. 
- DotNetCoreCLI task (`DotNetCoreCLI@2`) (`publish`) - This publishes the application to a specified output directory. 
- PublishBuildArtifacts task (`PublishBuildArtifacts@1`) - This publishes the contents of the output directory as a build artifact. This task publishes the build artifacts to the artifact staging directory, which is a temporary location on the build agent. The artifacts can then be downloaded by other tasks and used in subsequent stages.
- DownloadBuildArtifacts task (`DownloadBuildArtifacts@0`) - This downloads the build artifact to a specified directory. This task downloads the build artifacts from the artifact staging directory. It is used in subsequent stages to deploy the application.
- AzureWebApp task (`AzureWebApp@1`) (`deploy`) - This deploys the contents of the build artifact to an Azure Web App. This task deploys the .NET Core application to an Azure Web App service. It requires an Azure subscription and a connection to the Azure Web App service.

Watch this hands-on video here: https://youtu.be/TdvpZr5FYpk

That's it! This project is now complete.

