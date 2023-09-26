# Configure an Azure Monitor Alert Rule
 
## Introduction
 
In this lab, you will create an Azure Monitor alert rule on a storage account, using Azure CLI. Students with prior experience in Azure and working with Azure CLI will have the best opportunity to complete the lab objectives with minimal assistance. However, there are hints and tips to guide you, along with the detailed lab guide and solution video. So, even students new to the technology and concepts should be able to successfully complete the lab objectives.
 
## Solution
 
### Log In to the Azure Portal
 
1. Log in to the Azure portal using the credentials provided on the lab page. Be sure to use an incognito or private browser window to ensure you're using the lab account rather than your own.
2. Once logged in, click the Cloud Shell icon (**>_**) to the immediate right of the search bar on top of the portal.
3. When Cloud Shell opens, select **Bash**.
4. Select **Show advanced settings**.
5. Under **Storage account**, select **Create new** if it isn't already selected.
6. Under **Storage account**, enter *storage* followed by a string of random characters to make it globally unique.
7. Under **File share**, enter *cloudshell*.
8. Click the **Create storage** button. It may take a few minutes to create the new storage account.

### Use Azure CLI to Configure an Alert Rule
 
1. In the Azure portal on top of the terminal, select the storage account whose name starts with **pslab**.
2. Next to **Resource group**, copy the resource group name.
3. Return to the Cloud Shell terminal at the bottom and create an environment variable that sets the resource group name equal to the value that was just copied, pasting in the resource group name in the command where it says `INSERT-RESOURCE-GROUP-NAME`:

    ```
    resource_group_name=INSERT-RESOURCE-GROUP-NAME
    ```

4. In the Azure portal on top of the terminal, in the lefthand navigation menu, select **Settings**.
5. In the lefthand navigation menu, select **Endpoints**.
6. Copy the ID next to **Storage account resource ID**.
7. Create an environment variable that sets the scope for the alert rule equal to the storage account resource ID, pasting in the ID in the command where it says `INSERT-STORAGE-ACCOUNT-RESOURCE-ID`:

    ```
    rule_scope=INSERT-STORAGE-ACCOUNT-RESOURCE-ID
    ```

8. Create an environment variable for the action group name:

    ```
    action_group_name="my-agl"
    ```

9. Create the action group, entering in your own first name where it says `INSERT-YOUR-NAME` and entering in your phone number where it says `INSERT-YOUR-PHONE-NUMBER`:

    ```
    az monitor action-group create. \
    --name $action_group_name \
    --short-name $action_group_name \
    --resource-group $resource_group_name \
    --action sms INSERT-YOUR-NAME 1 INSERT-YOUR-PHONE-NUMBER
    ```
    
    You should receive a text message confirming the action group has been created.


10. Create an activity log alert:

    ```
    az monitor activity-log alert create \
      --name "myalertrule1" \
      --resource-group $resource_group_name \
      --scope $rule_scope \
      --condition "Category=Administrative and OperationName=Microsoft.Storage/storageAccounts/write" \
      --description "my description" \
      --action-group $action_group_name
    ```
    
    You should receive a JSON description of your alert.

11. List the alerts to double-check that the activity log alert was successfully created:

    ```
    az monitor activity-log alert list
    ```

12. Return to the Azure portal on top of the terminal, and in the lefthand navigation menu, select **Alerts**.
13. Select **Alert rules**. You should see the alert rule you just created.
 
## Conclusion
 
Congratulations — you've completed this hands-on lab!