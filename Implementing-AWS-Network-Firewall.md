# Implementing AWS Network Firewall
 
## Introduction
 
In this lab, we will be deploying AWS Network Firewall to a VPC and then configuring the environment to allow an EC2 instance access to a web page on the internet. To complete this lab, you must be familiar with the AWS Management Console and understand what the AWS Network Firewall is and the capabilities it has to offer.
 
## Solution
 
Log in to the AWS Management Console using the credentials provided on the lab instructions page. Make sure you're using the `us-east-1` Region.
 
### Create Firewall Subnet in VPC
 
1. In the search bar on top of the console, enter *vpc*.
2. From the search results, select **VPC**.
3. In the lefthand navigation menu, under **Virtual private cloud**, select **Your VPCs**.
4. Check the box next to **FirstVPC** to select it.
5. Select the **Resource map** tab. This will show you what resources have already been configured for the VPC.
6. In the lefthand navigation menu, select **Subnets**.
7. Click the **Create subnet** button.
8. On the **Create subnet** page, set the following parameters:
  - **VPC ID**: Select the first VPC from the dropdown menu.
  - **Subnet name**: Enter *FirewallSubnet*.
  - **Availability Zone**: Select **US East (N. Virginia) / us-east-1a** from the dropdown menu.
  - **IPv4 CIDR block**: Enter *10.0.0.0/28*.
9. Click the **Create subnet** button.
10. Once the subnet has been created, check the box next to **FirewallSubnet** to select it.
11. Select the **Route Table** tab.
12. Click the **Edit route table association** button.
13. Under **Route table ID**, select the route table labeled **FirewallSubnetRouteTable** from the dropdown menu.
14. Confirm that the 0.0.0.0/0 route with a target of the internet gateway (igw) is available under **Destination**.
15. Click the **Save** button. This should successfully create the subnet.

### Create Network Firewall Rule Group
 
1. In the lefthand navigation menu, under **Network Firewall**, select **Network Firewall rule groups**.
2. Click the **Create Network Firewall rule group** button.
3. On the **Create Network Firewall rule group** page, set the following parameters:
  - **Name**: Enter *WebsiteWhiteList*.
  - **Capacity**: Enter *10*.
  - **Stateful rule group options**: Select **Domain list**.
  - **Domain list**: Enter *.acloudguru.com*.
 4. Click the **Create stateful rule group** button. This should successfully create the stateful rule group.

### Create Firewall Policy

1. In the lefthand navigation menu, under **Network Firewall**, select **Firewall policies**.
2. Click the **Create firewall policy** button.
3. Under **Name**, enter *testfirewallpolicy*.
4. Click the **Next** button.
5. Under **Stateful rule group**, click the **Add stateful rule groups** button.
6. Check the box next to **WebsiteWhiteList**, the stateful rule group that was previously created, to select it.
7. Click the **Add rule groups** button.
8. Click the **Next** button.
9. Click the **Next** button.
10. Click the **Next** button.
11. Click the **Create firewall policy** button. This should successfully create the firewall policy.

### Create Network Firewall

1. In the lefthand navigation menu, under **Network Firewall**, select **Firewalls**.
2. Under **Get started**, click the **Create firewall** button.
3. On the **Create firewall** page, set the following parameters:
  - **Name**: Enter *TestNWFW*.
  - **VPC**: Select **FirstVPC** from the dropdown menu.
  - **Availability Zone**: Select **us-east-1a** from the dropdown menu.
  - **Subnet**: Select **FirewallSubnet** from the dropdown menu.
  - **IP address type**: Select **IPv4** from the dropdown menu.
  - **Associated firewall policy**: Select **Associate an existing firewall policy**.
  - **Firewall policy**: Select **testfirewallpolicy** from the dropdown menu.
4. Click the **Create firewall** button. It may take a few minutes for the firewall to finish being provisioned, and you may need to refresh the page to see the completed firewall.

### Reconfigure Route Tables to Permit Sending Traffic Destined for the Internet to the Network Firewall

#### Create Outbound Route 

1. In the lefthand navigation menu, under **Virtual private cloud**, select **Route tables**.
2. Check the box next to **WorkloadRouteTable** to select it.
3. Select the **Routes** tab.
4. Click the **Edit routes** to add a new route.
5. Click the **Add route** button.
6. In the **Destination** column, enter *0.0.0.0/0* to create a catch-all route.
7. In the **Target** column, select **Gateway Load Balancer Endpoint** from the dropdown menu. This should create a new endpoint.
8. Click the **Save changes** button.

#### Create Return Route

1. In the lefthand navigation menu, select **Route tables**.
2. Check the box next to **InternetRouteTable** to select it.
3. Select the **Routes** tab.
4. Click the **Edit routes** to add a new route.
5. Click the **Add route** button.
6. In the **Destination** column, enter *10.0.1.0/24* to create a catch-all route.
7. In the **Target** column, select **Gateway Load Balancer Endpoint** from the dropdown menu. This should create a new endpoint.
8. Click the **Save changes** button.
9. Select the **Edge associations** tab.
10. Click the **Edit edge association** button.
11. Click the checkbox in the **Internet gateway** box to select it as the associated internet gateway.
12. Click the **Save changes** button. This will complete the configuration of the network firewall.

### Test Access from EC2 Instance

 1. In the search bar on top of the console, enter *ec2*.
 2. From the search results, select **EC2** and choose to open it in a new browser tab or window.
 3. Under **Resources**, select **Instances (running)**.
 4. Click the checkbox next to **EC2Instance1** to select it.
 5. Click the **Connect** button.
 6. Select the **Session Manager** tab.
 7. Click the **Connect** button. This should open a new terminal session.
 8. In the terminal, check that a known whitelisted site is allowed:
 
    ```
    curl https://acloudguru.com
    ```
    
    This should show output of the website's text.
 
 9. Check that a different site not ending in `acloudguru.com` is disallowed by the firewall:
 
    ```
    curl https://www.bbc.co.uk
    ``` 
    
    This should show no output at all, as the website has been blocked by the firewall. Eventually, you should see a message that this connection has timed out. You can close the connection by entering `^C` before that happens.
 
## Conclusion
 
Congratulations — you've completed this hands-on lab!