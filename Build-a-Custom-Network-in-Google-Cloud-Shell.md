# Build a Custom Network in Google Cloud Shell
 
## Introduction
 
Networks are the very backbone of cloud computing, and the ability to create a custom network is crucial. Custom networks allow custom subnets and firewall rules as well, so you can completely control access to your resources. In this hands-on lab, you’ll use the Google Cloud Shell to create a Cloud VPC network with two subnets, firewall rules that allow SSH ingress, and Compute Engine VM instances that connect to the subnets. Once the network and resources are established, you’ll test the connectivity of the networks via an SSH terminal.
 
## Solution
 
 1. On the lab page, click **Open Link in Incognito Window** to copy the link for Google Cloud Platform. Then, paste and open the link in an incognito window or private browser session.
 1. Sign in to Google Cloud Platform using the credentials provided on the lab page.
 1. On the **Welcome to your new account** screen, review the text, and click **Accept**. 
 1. On the **Welcome Cloud Student!** screen, check to agree to the terms of service, and select your country of residence.
 1. Click **Agree and Continue**.
 
### Create Custom Network and Subnets in Cloud Shell
 
1. In the Google Cloud Platform console, select the **>_** icon (the **Activate Cloud Shell** icon) immediately to the right of the search bar on top. It may take a few minutes for Cloud Shell to spin up.
2. When the Cloud Shell alert appears, select **Continue**.
3. Create a network called `ps-network` with a custom subnet:

    ```
    gcloud compute networks create ps-network --subnet-mode custom
    ```

4. When the alert to authorize Cloud Shell appears, select **Authorize**. This will allow the new network to be created, though the process may take a few minutes.
5. Create the first subnet, which will be located in the US East region and needs to be part of the newly created `ps-network` network with an IP range of `10.0.1.0/24`:

    ```
    gcloud compute networks subnets create ps-subnet-us-east --network ps-network --region us-east1 --range 10.0.1.0/24
    ```

6. Create the second subnet, which will be located in the Europe West region and needs to be part of the `ps-network` network with an IP address of :

    ```
    gcloud compute networks subnets create ps-subnet-eu-west --network ps-network --region europe-west1 --range 10.0.2.0/24
    ```

7. List the network and subnets that we just created:

    ```
    gcloud compute networks subnets list --network ps-network
    ```

### Define Firewall Rule to Allow SSH
 
Create a firewall rule to allow TCP access on port 22 and ICMP access, so that we can SSH into our instances:

    ```
    gcloud compute firewall-rules create ps-allow-ssh --allow tcp:22,icmp --network ps-network
    ```
    
It may take a few minutes to create the new firewall rule.

### Spin Up VM Instances
 
1. Create a VM instance in the U.S. East 1 zone named `ps-vm-us` for the `ps-subnet-us-east` subnet with the N2 machine type:

    ```
    gcloud compute instances create ps-vm-us --subnet ps-subnet-us-east --zone us-east1-b --machine-type=n2-standard-2
    ```
   
2. Create a VM instance in the Europe West 1 zone named `ps-vm-eu` for the `ps-subnet-eu-west` subnet with the N2 machine type:

    ```
    gcloud compute instances create ps-vm-eu --subnet ps-subnet-eu-west --zone europe-west1-b --machine-type=n2-standard-2
    ```
 
### Test via SSH

1. In the Google Cloud Platform console above Cloud Shell, select the hamburger menu icon (the icon that looks like three horizontal lines) in the top left corner of the console.
2. In the lefthand navigation menu, select **Compute Engine**.
3. Select **VM instances**. This should take you to the **VM instances** page, where you should see both VMs listed.
4. For the **ps-vm-eu** instance, copy the IP address listed under **External IP**.
5. For the **ps-vm-us** instance, select **SSH** under **Connect**. A new browser window or tab will open.
6. When prompted to authorize connecting to the VM, select **Authorize**. This will open an SSH terminal.
7. In the SSH terminal, ping the VM instance in Europe externally three times, pasting in the external IP for the Europe instance in `[EUROPE_VM_EXTERNAL_IP]`:

    ```
    ping -c 3 [EUROPE_VM_EXTERNAL_IP]
    ```

8. Return to the browser window or tab with the **VM instances** page open.
9. For the **ps-vm-eu** instance, copy the IP address listed under **Internal IP**.
10. Return to the SSH terminal.
11. Ping the VM instance in Europe internally three times, pasting in the internal IP for the Europe instance in `[EUROPE_VM_INTERNAL_IP]`:

    ```
    ping -c 3 [EUROPE_VM_INTERNAL_IP] 
    ```

## Conclusion
 
Congratulations — you've completed this hands-on lab!
