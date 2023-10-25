# Create a Stream Processing Solution in Azure Stream Analytics
 
## Introduction
 
Imagine you work at your local arcade hall: a cool environment with lots of vintage games and excited customers. Besides operating the machines and talking to customers, you are also the designated IT person.

In that role, your boss has asked you to write all the game scores, which are send to an event hub, to an Azure storage account as well. He wants you to do this using a Stream Analytics copy query.
 
## Solution
 
Log in to the Azure portal using the credentials provided on the lab page. Be sure to use an incognito or private browser window to ensure you're using the lab account rather than your own.
 
### Create Event Hub Namespace with Event Hubs
 
1. Select **+ Create**.
2. In the search bar on the **Marketplace** page, enter *event hub*.
3. If you see a **Private Offers are now here** pop-up, click the **Close** button in the pop-up.
4. Select **Event Hubs** from the marketplace options.
5. On the **Event Hubs** page, click the **Create** button.
6. In the **Create Namespace** page, under the **Basics** tab, set the following values:
  - **Namespace name**: Enter *game-scores-* followed by your initials or a random series of characters and numbers to make the name globally unique.
  - **Pricing tier**: From the dropdown menu, select **Standard**.
7. Click the **Review + create** button.
8. Once the validation completes, click the **Create** button. It may take a few minutes for the namespace to be deployed.
9. Once the deployment is complete, click the **Go to resource** button.
10. In the lefthand navigation menu, select **Event Hubs**.
11. Select **+ Event Hub**.
12. On the **Create Event Hub** page, in the **Basics** tab, in **Name**, enter *game-scores*.
13. Click the **Review + create** button.
14. Click the **Create** button to create the event hub.

### Configure Stream Analytics Job with an Input, Output, and Query
 
1. In the lefthand navigation menu, select **Overview**.
2. Select the link next to **Resource group**.
3. Select **+ Create**.
4. In the search bar on the **Marketplace** page, enter *stream analytics job*.
5. Select **Stream Analytics job**.
6. Click the **Create** button.
7. On the **New Stream Analytics** page, in the **Basics** tab, set the following values:
  - **Name**: Enter *copy-game-scores*.
  - **Streaming units**: Set to **1/3**.
8. Click the **Review + create** button.
9. After validation completes, click the **Create** button.
10. Once the Stream Analytics job has completed deploying, click the **Go to resource** button.
11. In the lefthand navigation menu, select **Inputs**.
12. Click **+ Add input**.
13. From the dropdown menu, select **Event hub**.
14. In the **Event Hub** pane on the right, set the following values:
  - **Input alias**: Enter *game-scores-eh*.
  - **Event Hub consumer group**: Select **Use existing**.
15. Click the **Save** button. It can take a few minutes for the input to be added and tested.
16. In the lefthand navigation menu, select **Outputs**.
17. Select **+ Add output**.
18. From the dropdown menu, select **Blob storage/ADLS Gen 2**.
19. In the **Blob storage/ADLS Gen2** pane on the right, set the following values:
  - **Output alias**: Enter *game-scores-storage*.
  - **Container**: Select **Create new** and enter *game-scores*.
20. Click the **Save** button. It may take a few minutes for the output to be added and tested.
21. In the lefthand navigation menu, select **Query**. You should see Azure has already provided an example query, so you don't need to do anything more.
22. In the lefthand navigation menu, select **Overview**.
23. Select **Start job**. This process can take a few minutes.

### Submit Example Data
 
 1. Select the link next to **Resource group**.
 2. Select the event hub namespace, **game-scores-<your initials>**.
 3. In the lefthand navigation menu, select **Event Hubs**.
 4. Select **game-scores**.
 5. In the lefthand navigation menu, select **Generate data (preview)**.
 6. Remove the example JSON and enter:

    ```
    "player": "<your name>",
    "score": 100
    ```
 
 7. Click the **Send** button.
 8. Expand the **Generate Data in quick steps to stream into Azure Event Hubs** option.
 9. In the JSON data, change the name next to `"player"` and the number next to `"score"`.
10. In the left, under **Repeat Send**, enter *3*.
11. Click the **Send** button. This should result in a total of four records in the storage account.

### Observe Results

1. In the lefthand navigation menu, select **Overview**.
2. Select the link next to **Resource group**.
3. Select the storage account link.
4. In the lefthand navigation menu, select **Containers**.
5. Select **gameo-scores**. You should see a file in the container.
6. Select the file.
7. Select **Edit**. You should see the four records that you sent to the event hub earlier, which confirms you are copying records from the event hub into blob storage using a functional streaming data solution.
 
## Conclusion
 
Congratulations — you've completed this hands-on lab!
