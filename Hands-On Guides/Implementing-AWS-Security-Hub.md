# Implementing AWS Security Hub
 
## Introduction
 
Globomantics is a global healthcare organization who has recently moved their key systems to AWS. Globomantics is finding it difficult to maintain adherence to certain industry and AWS standards. You have been asked to implement a solution that will:

- Automatically assess their AWS resources against the PCI DSS standard and the AWS Foundational Security Best Practices (FSBP) standard
- Provide notifications when the most urgent breaches of standards and best practices are registered

Having considered several options, you have decided to recommend that Globomantics implement AWS Security Hub.

In this lab, you will:

1. Create an SNS topic with an email subscriber.
2. Create an Amazon EventBridge rule with the SNS topic as the target.
3. Enable AWS Config.
4. Enable AWS Security Hub.
 
## Solution
 
Log in to the AWS Management Console using the credentials provided on the lab instructions page. Make sure you're using the `us-east-1` Region.
 
### Create an SNS Topic
 
1. In the search bar on top of the console, enter *simple notification service*.
2. From the search results, select **Simple Notification Service**.
3. In the **Create topic** box, under **Topic name**, enter *eMailMe*.
4. Click the **Next step** button.
5. On the **Create topic** page, select **Access policy - optional** to expand this section.
6. In the **Access policy - optional** section, set the following parameters:
  - **Publishers**: Select ***Everyone** from the dropdown menu.
  - **Subscribers**: Select **Everyone** from the dropdown menu.
7. Click the **Create topic** button.

### Create an Email Subscriber to the Topic
 
1. Click the **Create subscription** button.
2. On the **Create subscription** page, set the following parameters:
  - **Protocol**: Select **Email** from the dropdown menu.
  - **Endpoint**: Enter the email address to which you will be receiving email notifications.
3. Click the **Create subscription** button.
4. Go to your email address' inbox, where you should see a confirmation email. Please note, sometimes this email may take up to 15 minutes to arrive in your inbox.
5. In the email, select **Confirm subscription**.
6. Return to the AWS Management Console and refresh the subscription page. You should see under **Status** that the subscription should be **Confirmed**.

### Create an Amazon EventBridge Rule

1. In the search bar on top of the console, enter *amazon eventbridge*.
2. From the search results, select **Amazon EventBridge**.
3. Click the **Create rule** button.
4. On the **Create rule** page, set the following parameters:
  - **Name**: Enter *GloboSecurityHub*.
  - **Description - optional**: Enter *Rule that will send an email when a Security Hub finding is generated*.
5. Click the **Next** button.
6. On the **Build event pattern** page, set the following parameters:
  - **AWS service**: Select **Security Hub** from the dropdown menu.
  - **Event type**: Select **Security Hub Findings - Important**.
  -  **Event Type Specification 2**: Select **Specific Compliance status(es)**.
  -  **Specific Compliance status(es)**: Select **FAILED** from the dropdown menu.
  -  **Event Type Specification 5**: Select **Specific Record state(s)**.
  -  **Specific Record state(s)**: Select **ACTIVE** from the dropdown menu.
  -  **Event Type Specification 8**: Select **Specific Severity label(s)**.
  -  **Specific Severity label(s)**: Select **HIGH** and **CRITICAL** from the dropdown menu.
7. Click the **Next** button.
8. On the **Select target(s)** page, set the following parameters:
  - **Target 1**: Under **Target types**, select **AWS service**.
  - **Select a target**: Select **SNS topic** from the dropdown menu.
  - **Topic**: Select **eMailMe** from the dropdown menu.
9. Click the **Next** button.
10. Click the **Next** button.
11. Click the **Create rule** button.

### Enable and Configure AWS Config

1. In the search bar on top of the console, enter *config*.
2. From the search results, select **Config**.
3. In the **Set up AWS Config** box, click the **1-click setup** button.
4. On the **Review** button, click the **Confirm** button.

### Enable AWS Security Hub

#### Enable Security Hub with PCI DSS Compliance

1. In the search bar on top, enter *security hub*.
2. From the search results, select **Security Hub**.
3. Click the **Go to Security Hub** button.
4. On the **Enable AWS Security Hub**, under **Security standards**, select **Enable PCI DSS** in addition to **Enable AWS Foundational Security Best Practices** and **Enable CIS AWS Foundation Benchmark**.
5. Click the **Enable Security Hub**.
6. On the **Summary** page, under **Security standards**, select **PCI DSS** to see more information.
7. In the lefthand navigation menu, select **Findings**. This is where findings would be listed, although you may not see any findings listed right away.

#### Generate Sample Findings

1. To generate sample findings, in the search bar on top of the console, enter *ec2*.
2. From the search results, select **EC2** and open it in a new browser tab or window.
3. In the lefthand navigation menu, under **Network & Security**, select **Security Groups**.
4. Click the **Create security group** button.
5. On the **Create security group** page, set the following parameters:
  - **Security group name info**: Enter *GLOBOSecurityGroup*.
  - **Description**: Enter *A Globomantics Security Group*.
6. Click the **Add rule** button.
7. Under **Inbound rules**, set the following parameters:
  - **Type**: Select **All ICMP - IPv4** from the dropdown menu.
  - **Source**: Select **0.0.0.0/0**.
8. Click the **Add rule** button to add a second rule.
9. For the second rule, set the following parameters:
  - **Type**: Select **SSH** from the dropdown menu.
  - **Source**: Select **0.0.0.0/0**.
10. Click the **Add rule** button to add a third rule.
11. For the third rule, set the following parameters:
  - **Type**: Select **SSH** from the dropdown menu.
  - **Source**: Select **::/0**.
12. Click the **Add rule** button to add a fourth rule.
13. For the fourth rule, set the following parameters:
  - **Type**: Select **RDP** from the dropdown menu.
  - **Source**: Select **0.0.0.0/0**.
14. Click the **Add rule** button to add a fifth rule.
15. For the fifth rule, set the following parameters:
  - **Type**: Select **RDP** from the dropdown menu.
  - **Source**: Select **::/0**.
16. Click the **Create security group** button.
17. Go back to the browser tab or window with **Security Hub > Findings** open. Findings may take a few minutes to generate and it may be necessary to refresh the page a few times, but you should see a list of different findings appear.
18. Check your email inbox to see if you receive notifications about these findings.
 
## Conclusion
 
Congratulations — you've completed this hands-on lab!
