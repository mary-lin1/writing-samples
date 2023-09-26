# Deploying a Web App to Azure App Service
 
## Introduction
 
In this hands-on lab, you are working as a cloud engineer for Tech Tangos, an up-and-coming computer repair company. You’ve been asked to deploy a simple website to Azure.

You’ll use the Azure portal to deploy a sample website as a proof of concept that the Tech Tangos site can be hosted on Azure App Services.

 
## Solution
 
### Sign In to the Azure Portal
 
Log in to the Azure portal using the credentials provided on the lab page. Be sure to use an incognito or private browser window to ensure you're using the lab account rather than your own.

### Create a Linux .NET Core Azure App Service
 
1. Take note of the region (listed next to **Location**) where your resource group is located, so you can create your resources in the same region.
2. In the search bar on top of the portal, enter *app service*.
3. From the search results, select **App Services**.
4. Click **+ Create**.
5. Select **Web App**.
6. On the **Create Web App** page, under the **Basics** tab, set the following values:
  - **Resource group**: From the dropdown menu, select the existing resource group listed.
  - **Name**: Enter *cbwebapp001*.
  - **Runtime stack**: Select **.NET 7 (STS)**.
  - **Operating System**: Select **Linux**.
  - **Region**: Select the same region as the location of your resource group.
  - **Pricing plan**: Select **Basic B1**.
7. Click the **Review + create** button.
8. Click the **Create** button. This will take a few minutes to finish.

### Deploy a Web App to the App Service
 
1. In the lefthand navigation menu, under **Deployment**, select **Deployment Center**.
2. For **Source**, select **External Git** from the dropdown menu.
3. In **Repository**, paste in this link: https://github.com/Azure-Samples/dotnetcore-docs-hello-world.
4. In **Branch**, enter *master*.
5. At the top of the page, click **Save**. This can take a few minutes for the deployment to finish.
6. Select the **Logs** tab to view the progress of the deployment. You may need to wait several minutes and refresh the page before you see **Success (Active)** under the **Status** column.
7. At the top of the page, select **Browse** to visit the newly created web app. You should see a new web page open up with the text **Hello World from .NET 7**.
 
## Conclusion
 
Congratulations — you've completed this hands-on lab!