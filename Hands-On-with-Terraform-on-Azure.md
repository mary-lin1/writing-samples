# Hands-On with Terraform on Azure
 
## Introduction
 
In this hands-on lab, you'll create a Terraform module and publish the module to a private registry in Terraform Cloud. You'll also configure a GitHub repository and continuous delivery for a configuration that leverages the module to deploy a secure storage account.
 
## Solution

In this lab, you will be connecting to the virtual machine using Remote Desktop, where you will have access to the Azure Portal.

**Note:** To complete this lab, you will need to use a remote desktop client:

- Windows: [Microsoft Remote Desktop on Windows 10](https://support.microsoft.com/en-us/windows/how-to-use-remote-desktop-5fe128d5-8fb1-7a23-3b8a-41e636865e8c)
- MacOS: [Microsoft Remote Desktop](https://apps.apple.com/us/app/microsoft-remote-desktop-10/id1295203466?mt=12)
- Linux: [Remmina](https://remmina.org/how-to-install-remmina/)

To use the Remote Desktop client:

1. In the Azure portal, click **Virtual machines** in the left-hand menu.
1. Click the listed **lab-vm** virtual machine.
1. Click **Connect**.
1. Click **Download RDP File**.
1. Open the file in your Remote Desktop app.
1. Edit the desktop to:
    - Make sure you are _not_ in an admin session.
    - Make sure the remote session does _not_ launch in full-screen mode.
    - Set the resolution to something that will give you room to maneuver in both the remote and local desktops (e.g., 1024 x 768).
1. Log in to the virtual machine using the following credentials:
    - **User Name**: Use the user name provided in the lab credentials.
    - **Password**: Use the password provided in the lab credentials.

**Note**: If you're using a Mac, instead of downloading the RDP file, you will need to copy the public IP address of the virtual machine in the Azure Portal and use that to access the virtual machine via your RDP client.

To complete this lab, you will need:

* Your own free Terraform Cloud account. You can sign up at [Terraform Cloud](https://app.terraform.io/public/signup).
* Your own free GitHub account. You can sign up to [join GitHub](https://github.com/join).
 
### Create a GitHub Repository for Your Module
 
1. Once you've logged in to the virtual machine, open a new browser window. 
2. Log in to the Azure portal using the credentials provided on the lab page. Be sure to use an incognito or private browser window to ensure you're using the lab account rather than your own.
3. In a new browser window or tab, log in to your free account in Terraform Cloud.
4. In a new browser window or tab, log in to your GitHub account.
5. Near the top left of the page, click the **New** button next to **Top Repositories** to create a new repository.
6. On the **Create a new repository** page, set the following parameters:
  - **Owner**: Select yourself.
  - **Repository name**: Enter *terraform-azurerm-securestorage*.
  - **Public** Select the radio button for this option.
  - **Add a README file**: Check the checkbox for this option.
  - **Add gitignore**: Enter *terraform* and select **Terraform** from the dropdown menu.
7. Click the **Create repository** button.
8. Click the **<> Code** button.
9. Copy the HTTPS URL for the repository.

### Author Your Terraform Module
 
#### Clone Your GitHub Repository

1. Go to Visual Studio Code, which you can open by clicking on the blue Visual Studio Code icon in the Windows taskbar at the bottom of the screen.
2. Press **Ctrl + Shift + P** (or **Cmd + Shift + P**) to activate the Command Palette.
3. In the Command Palette, enter *git clone*.
4. Select **Git Clone** from the search results.
5. Paste in the repository URL from GitHub and press **Enter**.
6. Select the **Documents** folder.
7. Click the **Select as Repository Destination** button.
8. When asked if you would like to open the closed repository, click the **Open** button.
9. Click the **Yes, I trust the authors** button.

#### Author the Module

 1. In the left-hand pane, right-click and select **New File**. Alternatively, you can click the New File icon to create a new file.
 2. Name the new file *main.tf*.
 3. Right-click again and select **New File**.
 4. Name this new file *outputs.tf*.
 5. Right-click again and select **New File**.
 6. Name this third file *variables.tf*.
 7. Select **main.tf**.
 8. Right-click **variables.tf** and select **Open to the Side**.
 9. In `main.tf`, create a Terraform block:

    ```
    terraform {
      required_version = ">= 1.3.0”

      required_providers {
        azurerm = {
          source  = "hashicorp/azurerm"
          version = ">= 3.43.0"
        }
      }
    }
    ```

10. In the Visual Studio Code taskbar, select **Terminal**.
11. In the menu, select **New Terminal**.
12. In the terminal, initialize the working directory:

    ```
    terraform init
    ```

13. In `main.tf`, create the resource group block:

    ```
    resource "azurerm_storage_account" "securestorage" {
      resource_group_name = var.resource_group_name
      location = var.location
      name = var.storage_account_name
      account_tier = var.account_tier
      account_replication_type = var.account_replication_type
      public_network_access_enabled = false
    }
    ```

14. In `variables.tf`, create the configuration and variables:

    ```
    variable "resource_group_name" {
      type = string
    }

    variable "location" {
      type = string
    }

    variable "storage_account_name" {
      type = string
    }

    variable "account_tier" {
      type = string
      description = "The storage account tier: Standard or Premium"
      default = "Standard"
      # Note about validation
    }

    variable account_replication_type {
      type = string
      description = "The storage account replication type: LRS, GRS, RAGRS, ZRS, GZRS, RAGRS"
      default = "GRS"
    }
    ```

15. In `outputs.tf`, define an output:

    ```
    output "storage_account_id" {
      value       = azurerm_storage_account.securestorage.id
      description = "The storage account name"
    }
    ```

16. Save and close `main.tf` by pressing **Ctrl + S** (or **Cmd + S** on a Mac).
17. Save and close `variables.tf` by pressing **Ctrl + S** (or **Cmd + S** on a Mac).
18. Save and close `outputs.tf` by pressing **Ctrl + S** (or **Cmd + S** on a Mac).
19. In the terminal, make sure the files are formatted correctly:

    ```
    terraform format
    ```
    
20.  Validate the Terraform:

    ```
    terraform validate
    ```

20. Configure your Git user name:

    ```
    git config user.name "<your user name here>"
    ```

21. Config your Git email address:

    ```
    git config user.email "<your email address here>"
    ```

22. In the leftmost vertical pane in Visual Studio Code, select the Source Control icon.
23. Next to each file (`.terraform.lock.hd`, `main.tf`, `outputs.tf`, and `variables.tf`), click the **+** icon to stage each file.
24. In the message field above the **Commit** button, enter a commit message such as *Version 1*.
25. Click the **Commit** button.
26. Click the **Sync Changes** button.
27. When told this action will pull and push commits to and from "origin/main", click **OK**.
28. Open the pop-up that appears and click the **Sign in with your browser** button.
29. In the new browser window or tab that has just opened in response, click the **Continue** button. This should allow your authentication to succeed.

### Publish Your Module to Your Private Registry

#### Publish Your Module
 
1. Go to the browser window or tab with GitHub open and refresh the page. You should see your committed Terraform files listed.
2. Select the **tags** tab.
3. Click the **Create a new release** button.
4. Click the **Chose a tag** button.
5. Enter *1.0.0* in the field.
6. Select **+ Create new tag 1.0.0 on publish**.
7. Enter the name *1.0.0* for this release.
8. Enter a description of *First release*.
9. Click the **Publish release** button.
10. Go to the browser window or tab with Terraform Cloud open. 
11. In the left-hand navigation menu, select **Registry**.
12. Click the **Publish** button in the top right.
13. Select **Module** from the dropdown.
14. Under **Connect to a version control provider**, select **GitHub**. (If you have not configured GitHub as a version control provider, you can find instructions on how to do so in this [tutorial on connecting GitHub to Terraform](https://developer.hashicorp.com/terraform/tutorials/cloud/github-oauth)).
15. Under **Can't see your repository? Enter its ID below**, enter the name of your repository, which should be *<your GitHub user name>/terraform-azurerm-securestorage*.
16. Click the arrow button to the right.
17. Click the **Publish module** button.

#### Configure a Terraform Workspace For Your Configuration

1. In the left-hand navigation menu, select **Projects & workspaces**.
2. Click the **Create a workspace** button.
3. Select **API-driven workflow**.
4. Under **Workspace Name**, enter *Hands-On_With_Terraform_On_Azure*.
5. Click the **Create workspace** button.
6. In the left-hand navigation menu, select **Variables**.
7. Create the first environmental variable for the ARM subscription ID:
  - Click the **+ Add variable** button.
  - Select **Environment variable**.
  - Under **Key**, enter *ARM_SUBSCRIPTION_ID*.
  - Go to the browser window or tab with the Azure portal open and copy the ID next to **Subscription ID**.
  - Go back to the browser window or tab with Terraform Cloud open and paste the ID under **Value**.
  - Click the **Add variable** button.
8. Create an environment variable for the ARM client ID:
  - Click the **+ Add variable** button.
  - Select **Environment variable**.
  - Under **Key**, enter *ARM_CLIENT_ID*.
  - From the lab credentials, copy the ID under **Application Client ID** in the **Service Principal** box.
  - Go back to the browser window or tab with Terraform Cloud open and paste the ID under **Value**.
  - Click the **+ Add variable** button.
9. Create an environment variable for the ARM client secret:
  - Click the **+ Add variable** button.
  - Select **Environment variable**.
  - Under **Key**, enter *ARM_CLIENT_SECRET*.
  - From the lab credentials, copy the ID under **Secret** in the **Service Principal** box.
  - Go back to the browser window or tab with Terraform Cloud open and paste the ID under **Value**.
  - Click the **+ Add variable** button.
10. Create an environment variable for the ARM client ID:
  - Click the **+ Add variable** button.
  - Select **Environment variable**.
  - Under **Key**, enter *ARM_TENANT_ID*.
  - Go to the browser window or tab with the Azure portal open and enter *Azure Active Directory* in the search bar.
  - From the search results, select **Azure Active Directory**.
  - Copy the ID next to **Tenant ID**.
  - Go back to the browser window or tab with Terraform Cloud open and paste the ID under **Value**.
  - Click the **+ Add variable** button.
 
### Configure a GitHub Repository and Actions for Your Configuration

1. Go back to the browser window or tab with GitHub open.
2. In the breadcrumb trail on top of the page, click the link with your user name to navigate to the home page.
3. Select **Repositories**.
4. Click the **New** button.
5. On the **Create a new repository** page, set the following parameters:
  - **Owner**: Select yourself.
  - **Repository name**: Enter *Hands-On_with_Terraform_on_Azure*.
  - **Public**: Select the radio button next to this option.
  - **Add a README file**: Check the checkbox next to this option.
  - **Add gitignore**: Select **terraform** from the dropdown menu.
6. Click the **Create repository** button.
7. Select **Actions**.
8. In the search field under **Get started with GitHub Actions**, enter *Terraform*.
9. In the **Terraform** box, select **Configure**.
10. Scroll down and remove the `pull_request:` line from the code.
11. Scroll to the bottom and remove the `if: github.ref == 'refs/heads/"main"' && github.event_name == 'push'` line from the code.
12. In the upper right, click the **Start commit** button.
13. Click the **Commit new file** button.
14. Select **Settings**.
15. In the left-hand navigation menu, select **Secrets and variables**.
16. Select **Actions** underneath **Secrets and variables**.
17. Click **New repository secret**.
18. Under **Name**, enter *TF_API_TOKEN*.
19. Go back to the browser window or tab with GitHub open and click on the user profile icon in the left-hand navigation menu.
20. Select **User Settings**.
21. In the left-hand navigation menu, select **Tokens**.
22. Click the **Create an API token** button.
23. Under **Description**, enter *GitHub*.
24. Click the **Create API token** button.
25. Copy the API token and close the pop-up.
26. Go back to the browser window or tab with Terraform Cloud open and paste the API token under **Secret**.
27. Click the **Add secret** button.

### Author and Apply Your Configuration

#### Clone the Git Repository

1. Select **Code** near the upper left.
2. Click the **<> Code** button.
3. Copy the repository URL.
4. Open Visual Studio Code.
5. Open the Command Palette with **Ctrl + Shift + P** (or **Cmd + Shift + P** on a Mac).
6. In the Command Palette, enter *git clone* and select it from the results.
7. Paste the repository URL.
8. Select the **Documents** folder.
9. Click the **Select as Repository Destination** folder.
10. When asked if you would like to open the cloned repository, click the **Open** button.
11. Click **Yes, I trust the authors**.

#### Author the Configuration

 1. In the left-hand panel, right-click and select **New File** or click the **New File** icon.
 2. Name the file *main.tf*.
 3. Go to the browser window or tab with Terraform Cloud open and navigate home by clicking the **Home** icon in the top left corner.
 4. Select the **Hands-On_With_Terraform_on_Azure** workspace.
 5. Copy the example code of the Terraform cloud configuration.
 6. Go back to Visual Studio Code and paste the code in `main.tf`.
 7. In the Terraform block under `terraform {`, set the required providers:

    ```
    required_providers {
        azurerm = {
	         source  = "hashicorp/azurerm"
	         version = ">= 3.43.0"
	    }
    }
    ```

 8. Save the file by pressing **Ctrl + S** (or **Cmd + S** on a Mac).
 9. In the Visual Studio Code taskbar, select **Terminal**.
10. From the menu, select **New Terminal**.
``. In the terminal, log in to Terraform:

    ```
    terraform login
    ```
    
12. When prompted, enter *yes* to continue.
13. In the new browser window or tab that pops up, enter *Workstation* under **Description**.
14. Click the **Create API token** button.
15. Copy the generated token.
16. Return to Visual Studio Code and paste in the token after `Enter a value:`.
17. Once successfully logged in, initialize the working directory:

    ```
    terraform init
    ```

18. In `main.tf`, configure the provider block:

    ```
    provider "azurerm" {
	    features {}
	    skip_provider_registration = true
    }
    ```

19. Configure the code for the existing resource group:

    ```
    resource "azurerm_resource_group" {
        name = ""
        location = ""
    }
    ```

20. Go back to the browser window or tab with the Azure portal open and click on the **Home** link on top of the page.
21. Under **Resources**, select the resource group.
22. In the left-hand navigation menu, select **Properties**.
23. Copy the name under **Name**. 
24. Go back to Visual Studio Code and paste the name between the double quotes after `name =`.
25. Go back to the browser window or tab with the Azure portal open and copy the ID under **Location ID**.
26. Return to Visual Studio Code and paste the location between the double quotes after `location =`.
27. Go back to the browser window or tab with Terraform Cloud open and navigate home by clicking the **Home** icon in the upper left of the left-hand navigation menu.
28. In the left-hand navigation menu, select **Registry**.
29. Select the **securestorage** module.
30. On the right-hand pane, copy the configuration details under **Copy configuration details**.
31. Return to Visual Studio Code and paste in the code under `main.tf`.
32. Initialize the working directory and download the module from Terraform Cloud:

    ```
    terraform init
    ```

33. Modify the module block in `main.tf` into this:

    ```
    module "securestorage" {
      source               = "app.terraform.io/ps-wayne-hoggett/securestorage/azurerm"
      version              = "1.0.0"
      location             = azurerm_resource_group.rg.location
      resource_group_name  = azurerm_resource_group.rg.name
      storage_account_name = "<Insert a globally unique name here>"
    }
    ```
    
    Make sure you use a globally unique name for the storage account name.

34. Save the file by pressing **Ctrl + S** (or **Cmd + S** on a Mac). You can also 
35. Close the `main.tf` file.

#### Apply the Configuration

 1. Format the Terraform file:
 
    ```
    terraform fmt
    ```
 
 2. Make sure the Terraform file is valid:
 
    ```
    terraform validate
    ```
 
 3. Log in using the Azure CLI:
 
    ```
    az login
    ```
    
    This will open a new browser window or tab with Microsoft Azure.
 
 4. In the new browser window or tab, select **Cloud Student** under **Pick an account**. 
 5. Go to the browser window or tab with the Azure portal open.
 6. Copy the ID under **Resource ID**.
 7. Import the existing resource group from Azure:
 
     ```
     terraform import azurerm_resource_group.rg <INSERT RESOURCE ID HERE>
     ```
    
 8. Paste in the resource ID at the end.
 9. Configure your user name on GitHub:
  
     ```
     git config user.name "<YOUR NAME HERE>"
     ```
 
10. Configure your email address on GitHub:
 
     ```
     git config user.email "<YOUR EMAIL HERE>"
     ```
 
11. In the leftmost vertical pane on Visual Studio Code, select the **Source Control** icon.
12. Stage both files (`.terraform.lock.hd` and `main.tf`) by selecting the **+** icon next to the file name.
13. In the field above the **Commit** button, enter *adding infrastructure* as a commit message.
14. Click the **Commit** button.
15. Click the **Sync CHanges** button.
16. When told that this action will pull and push comments from and to "origins/main", click the **OK** button.
17. Go back to the browser window or tab with GitHub open.
18. Select **Actions**.
19. Select **adding infrastructure**.
20. Select the **Terraform** job. The workflow may take a moment to complete.
21. Go back to the browser window or tab with the Azure portal open.
22. In the breadcrumb trail on top, select the resource group link to navigate back to the resource group.
23. Click **Refresh**. You should see that the storage account has been provisioned successfully.
 
## Conclusion
 
Congratulations — you've completed this hands-on lab!