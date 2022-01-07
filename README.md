## Deploy Linux .Net 6 Function App via Azure DevOps

### Pre-requisites:
1. **Function App**: [XingyiNet6FA](https://ms.portal.azure.com/#@microsoft.onmicrosoft.com/resource/subscriptions/83e0d97e-09ce-4ef1-b908-b07072b805e3/resourcegroups/LinuxConsumption/providers/Microsoft.Web/sites/XingyiNet6FA/appServices) | Linux .Net6 | Consumption Plan | West US 
2. **Runtime Version**: ~4 (per [supported languages for each runtime version](https://docs.microsoft.com/en-us/azure/azure-functions/functions-versions?tabs=in-process%2Cv4&pivots=programming-language-csharp#languages), your function app needs to be on v4 to host a .Net 6 application.)
3. **Working DevOps Repo**: [XingyiNet6FA](https://dev.azure.com/xingyifakedevops/_git/XingyiNet6FA) (imported from github repo https://github.com/Xingyixzhang/DotNet6ADOtestFA.git)

### Step-By-Step Instructions
Once a working project has been imported to your repositiry, let's build the project with the targeted .Net 6 SDK:
- Go to your Pipelines and click on **New Pipeline**;
- Choose **Use the classic editor** -- **Azure Repos Git** then select continue;
- Search for the template **Azure Functions for .Net** and click **Apply**;
- In the build pipeline configuration, name your pipeline and change the *Agent Specification* to **ubuntu-latest**;
- On the *Agent Job 1* click on **"+"** to create a new task for **Use .NET Core**;
- Select **SDK (contains runtime)** for *package to install*;
- Give the value of **6.x** to *Version* to specify the .Net Core SDK version in use;
- Move this task right before the *Build Project* task;
- Change the display names for all your tasks as needed or required;
- Click **Save & Queue** to run your build pipeline.

Now that you have successfully built the project, let's create a release pipeline to deploy your app to Azure Functions:
- Go to **Releases** under Azure DevOps Pipelines and click **Create New Release Pipeline**;
- Search for the template **Deploy a function app to Azure Functions** and click **Apply**;
- Change the stage name as needed, or leave as is for "Stage 1";
- Click on **Add an Artifact** and choose **Build** as the source type;
- Select the successful build pipeline you created earlier for your current project and click **Add**;
- Click on **1 job, 1 task** in your created stage to configure deployment details;
- Fill in the values for your subscription, choose **Function App on Linux**, and select your destination function app name;
- Click on **Run on Agent** and change the *Agent Specification* to **ubuntu-latest**;
- Click on the **Deploy Azure Function App** task and leave the *Runtime stack* **blank** as the drop down doesn't have the option for .Net 6;
- Click on **Save** and leave the folder as is;
- Click on **New Releae Pipeline** on the top to rename the release and on the top right, click **Create Release**;
- Choose the stage we have just configured from the pipeline drop down and click **Create**;
- Follow the release link and hover mouse over the stage to click on the **Deploy** button;
- Click on **Logs** to view the details as your app deploys to Azure Functions.

For troubleshooting purposes, you can also [run your release in debug mode](https://docs.microsoft.com/en-us/azure/devops/pipelines/release/variables?view=azure-devops&tabs=batch#run-a-release-in-debug-mode).
