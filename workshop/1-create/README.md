![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)
# **Amazon Elastic File System (Amazon EFS)**

## Create

### Version 2018.11

efs.wrkshp.2018.11

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

Errors or corrections? Email us at [darrylo@amazon.com](mailto:darrylo@amazon.com).

---

### Table of Contents  


[1. Create an Amazon VPC](#step-1)

[2. Create an Amazon EFS file system](#step-2) 

[3. Confirm SNS subscription](#step-3)

[4. Verify CloudWatch alarms](#step-4)

[5. Verify the EC2 instances terminates](#step-5)

[6. Next tutorial](#next-tutorial)

[7. Bonus](#bonus)

 [- Add data to grow a file system](#add-data-to-grow-a-file-system)

 [- Create an AWS CloudWatch Dashboard and Alarms to a file system](#create-an-aws-cloudwatch-dashboard-and-alarms-to-a-file-system)

 [- Create AWS CloudWatch Alarms to a file system](#create-aws-cloudwatch-alarms-to-a-file-system)

 [- Create an AWS CloudWatch Dashboard for a file system](#create-an-aws-cloudwatch-dashboard-for-a-file-system)

---

## Tutorial Overview

### Overview

This workshop is designed to create an Amazon EFS environment for subsequent sections which are designed to help you better understand the distributed data storage design of Amazon Elastic File System (Amazon EFS) and how to best leverage this design by taking advantage of scale-out achitectures.

### Prerequisites

* An AWS account with administrative level access
* An Amazon EC2 key pair

If a key pair has not been previously created in your account, please refer to [Creating a Key Pair Using Amazon EC2](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#having-ec2-create-your-key-pair) from the AWS EC2 User's Guide.  

Verify that the key pair is created in the same AWS region you will use for the tutorial.

WARNING!! This tutorial environment will exceed your free-usage tier. You will incur charges as a result of building this environment and executing the scripts included in this tutorial. Delete all files on the EFS file system that were created during this tutorial and delete the  stack so you don’t continue to incur additional compute and storage charges.

## Workshop: Create an Amazon Elastic File System (Amazon EFS) file system

### Step 1: Identify a VPC where you want to create an EFS file system

- Identify an existing VPC id (either one you had created prior to starting this workshop or the VPC Id from VPC1 created from the prerequisites CloudFormation stack)

### Step 2: Create a file system using the Amazon EFS Management Console

- Click on the link below to log in to the Amazon EFS Management Console in the same AWS region where you created your VPCs. 


| AWS Region Code | Region Name |
| :--- | :--- 
| us-east-1 | [US East (N. Virginia)](https://console.aws.amazon.com/efs/home?region=us-east-1#/wizard/1) |
| us-east-2 | [US East (Ohio)](https://console.aws.amazon.com/efs/home?region=eu-west-1#/wizard/1) |
| us-west-1 | [US West (N. California)](https://console.aws.amazon.com/efs/home?region=eu-west-1#/wizard/1) |
| us-west-2 | [US West (Oregon)](https://console.aws.amazon.com/efs/home?region=eu-west-1#/wizard/1) |
| ap-northeast-2 | [Asia Pacific (Seoul)](https://console.aws.amazon.com/efs/home?region=eu-west-1#/wizard/1) |
| ap-southeast-1 | [Asia Pacific (Singapore)](https://console.aws.amazon.com/efs/home?region=eu-west-1#/wizard/1) |
| ap-southeast-2 | [Asia Pacific (Sydney)](https://console.aws.amazon.com/efs/home?region=eu-west-1#/wizard/1) |
| ap-northeast-1 | [Asia Pacific (Tokyo)](https://console.aws.amazon.com/efs/home?region=eu-west-1#/wizard/1) |
| eu-central-1 | [EU Central (Frankfurt)](https://console.aws.amazon.com/efs/home?region=eu-west-1#/wizard/1) |
| eu-west-1 | [EU East (Ireland)](https://console.aws.amazon.com/efs/home?region=eu-west-1#/wizard/1) |


### Create Amazon Elastic File System (Amazon EFS)

#### Overview

This AWS Cloudformation template, and nested templates, will create an Amazon EFS file system and other AWS resources to monitor and send notifications if the burst credit balance of the file system drops below predefined thresholds. These alarms and other AWS CloudWatch metrics, including a file system size custom metric are added as widgets to a CloudWatch dashboard.

#### Parameters

- Add 100 GiB of data

- Select an existing key pair

- Select the default security group of the VPC created in Step 1 above

- Select three (3) Availability Zones of the VPC created in Step 1 above

- Add an email address to the SNS Email Address field that will receive AWS CloudWatch alarm notifications

- Accept all other parameter defaults

---

![](/images/deploy_to_aws.png)

---

---

### Step 3:
### Confirm SNS subscription

The email address entered as an input parameter will automatically be subscribed to the SNS topic and will receive an **AWS Notification - Subscription Confirmation email**. Click on the ***Confirm subscription*** link in the email.

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/efs-burst-credit-balance-notifications-confirm-subscription.png)

---

### Step 4:
### Verify CloudWatch alarms have been created

Verify all four CloudWatch alarms have been created. See the **AWS Resources** section below for the alarm names.

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-create-alarms.png)

### Step 5:
### Verify the EC2 instances terminate after a few minutes

Two instances are launched from separate Auto Scaling groups.

- The EC2 instance will automatically terminate once the script runs that calcualtes the burst credit balance thresholds.
- The EC2 instance will automatically terminate once the script runs that adds data to grow the file system.


### AWS Resources

#### Amazon EFS file system

One EFS file system is created within the region and mount targets in the subnets selected as parameters.  Data to grow the file system is automatically added using an EC2 instance launched from an Auto Scaling group. 

#### Custom CloudWatch metric for file system size

An AWS Lambda function is created and scheduled to run every minute which gets the metered size of the file system, from the describe-file-system command, and adds the value as a custom metric to CloudWatch.

#### SNS Topic w/ the email address subscribed

One SNS topic and subscription that receives all SNS notifications.

>Naming convention: {file-system-id}-notifications-{cloudformation-stack-name}

>```example: fs-26e6418e-notifications-efs-burst-credit-balance-notifications-6ECABFA1```

#### CloudWatch Alarms

Four CloudWatch alarms.

- One 'Warning' alarm that alerts when the burst credit balance drops below the 'warning' threshold for 5 minutes.

- One 'Critical' alarm that alerts when the burst credit balance drops below the 'critical' threshold for 5 minutes.

- One burst credit balance increase threshold alarm that alerts when the permitted throughput increases for 5 minutes.

- One burst credit balance decrease threshold alarm that alerts when the permitted throughput decreases for 5 minutes.

CloudWatch Alarm Examples:

| Warning Alarm | Critical Alarm | Permitted Throughput Increase | Permitted Throughput Decrease |
| --- | --- | --- | ---
| ![burst-credit-balance-warning](https://s3.amazonaws.com/aws-us-east-1/tutorial/burst-credit-balance-warning-screenshot.png) | ![burst-credit-balance-critical](https://s3.amazonaws.com/aws-us-east-1/tutorial/burst-credit-balance-critical-screenshot.png) | ![permitted-throughput-increase](https://s3.amazonaws.com/aws-us-east-1/tutorial/permitted-throughput-increase-screenshot.png) | ![permitted-throughput-decrease](https://s3.amazonaws.com/aws-us-east-1/tutorial/permitted-throughput-decrease-screenshot.png) |

#### CloudWatch Dashboard

A CloudWatch dashboard is created with widgets for key metrics, the burst credit balance alarms, and file systme size custom metric.

CloudWatch Dashboard Example:

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-create-dashboard.png)

#### IAM Policy & EC2 Instance Profile

One IAM policy and EC2 instance profile attached to the auto scaling launch configuration. This grants API permissions for the script to run.

| Allows |
| --- |
| cloudwatch:GetMetricStatistics |
| cloudwatch:PutMetricAlarm |
| autoscaling:DescribeAutoScalingGroups |
| autoscaling:DescribeAutoScalingInstances |
| autoscaling:UpdateAutoScalingGroup |
| elasticfilesystem:DescribeFileSystems |
| sns:Publish |

>```Policy name: efs-burst-credit-balance-cloudwatch-alarms```

#### Auto Scaling Group & Launch Configuration to add data

One auto scaling group and launch configuration to launch an EC2 instance that runs a script to add data that grows the file system.

>The auto scaling group will have a maximum size of 1, a desired size of 1, and a minimum size of 0.


#### Auto Scaling Group & Launch Configuration dynamically calculate burst credit balance thresholds

One auto scaling group and launch configuration to launch an EC2 instance that runs a script to calculate the burst credit balance thresholds.

>The auto scaling group will have a maximum size of 1, a desired size of 1, and a minimum size of 0.

>The launch configuration will launch instances with no EC2 key-pair and the security group created above. A script is dynamically generated in the /tmp directory. This script will run at boot-time and will calculate the burst credit balance thresholds.


---
## Next tutorial
### Click on the link below to go to the next Amazon EFS tutorial

| Tutorial | Link
| --- | ---
| **Performance** | [![](/images/efs_tutorial.png)](/tutorial/performance) |

---
## Bonus
### Individual CloudFormation Templates for existing Amazon EFS file systems

Below are the unnested CloudFormation templates from above that can be run individually against an existing file system. This allows you to add the features highlighted in this tutorial to an existing file system, like...

- adding data to grow a file system
- creating CloudWatch alarms to monitor burst credit balance and a CloudWatch dashboard with widgets for the burst credit balance alarms, and other important file system metrics, including a custom metric for file system size (this has the next two tools combined into one)
- creating just the CloudWatch alarms to monitor burst credit balance
- creating just the CloudWatch dasboard with widgets for important file system metrics, including a file system size custom  metric


### Add data to grow a file system

| AWS Region Code | Name | Launch |
| --- | --- | --- 
| us-east-1 |US East (N. Virginia)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=efs-add-data&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-add-data.yml) |
| us-east-2 |US East (Ohio)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=efs-add-data&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-add-data.yml) |
| us-west-2 |US West (Oregon)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=efs-add-data&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-add-data.yml) |
| eu-west-1 |EU (Ireland)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=efs-add-data&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-add-data.yml) |
| eu-central-1 |EU (Frankfurt)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?stackName=efs-add-data&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-add-data.yml) |
| ap-southeast-2 |AP (Sydney)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/new?stackName=efs-add-data&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-add-data.yml) |

### Create an AWS CloudWatch Dashboard and Alarms to a file system

Creates an AWS CloudWatch Dashboard with a file system size custom metric and CloudWatch Alarms to monitor Burst Credit Balance to an existing file system. This template is a combination of the following two templates.

| AWS Region Code | Name | Launch |
| --- | --- | --- 
| us-east-1 |US East (N. Virginia)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=efs-create-dashboard-alarms&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-dashboard-with-size-monitor-and-burst-credit-balance-alarms.yml) |
| us-east-2 |US East (Ohio)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=efs-create-dashboard-alarms&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-dashboard-with-size-monitor-and-burst-credit-balance-alarms.yml) |
| us-west-2 |US West (Oregon)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=efs-create-dashboard-alarms&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-dashboard-with-size-monitor-and-burst-credit-balance-alarms.yml) |
| eu-west-1 |EU (Ireland)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=efs-create-dashboard-alarms&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-dashboard-with-size-monitor-and-burst-credit-balance-alarms.yml) |
| eu-central-1 |EU (Frankfurt)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?stackName=efs-create-dashboard-alarms&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-dashboard-with-size-monitor-and-burst-credit-balance-alarms.yml) |
| ap-southeast-2 |AP (Sydney)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/new?stackName=efs-create-dashboard-alarms&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-dashboard-with-size-monitor-and-burst-credit-balance-alarms.yml) |

### Create AWS CloudWatch Alarms to a file system

Creates AWS CloudWatch Alarms to monitor Burst Credit Balance of an existing file system.

| AWS Region Code | Name | Launch |
| --- | --- | --- 
| us-east-1 |US East (N. Virginia)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=efs-create-alarms&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-burst-credit-balance-alarms.yml) |
| us-east-2 |US East (Ohio)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=efs-create-alarms&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-burst-credit-balance-alarms.yml) |
| us-west-2 |US West (Oregon)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=efs-create-alarms&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-burst-credit-balance-alarms.yml) |
| eu-west-1 |EU (Ireland)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=efs-create-alarms&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-burst-credit-balance-alarms.yml) |
| eu-central-1 |EU (Frankfurt)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?stackName=efs-create-alarms&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-burst-credit-balance-alarms.yml) |
| ap-southeast-2 |AP (Sydney)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/new?stackName=efs-create-alarms&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-burst-credit-balance-alarms.yml) |



### Create an AWS CloudWatch Dashboard for a file system

Creates an AWS CloudWatch Dashboard with a file system size custom metric for an existing file system.

| AWS Region Code | Name | Launch |
| --- | --- | --- 
| us-east-1 |US East (N. Virginia)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=efs-create-dashboard&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-dashboard-with-size-monitor.yml) |
| us-east-2 |US East (Ohio)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/new?stackName=efs-create-dashboard&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-dashboard-with-size-monitor.yml) |
| us-west-2 |US West (Oregon)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/new?stackName=efs-create-dashboard&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-dashboard-with-size-monitor.yml) |
| eu-west-1 |EU (Ireland)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/new?stackName=efs-create-dashboard&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-dashboard-with-size-monitor.yml) |
| eu-central-1 |EU (Frankfurt)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/new?stackName=efs-create-dashboard&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-dashboard-with-size-monitor.yml) |
| ap-southeast-2 |AP (Sydney)| [![cloudformation-launch-stack](/images/deploy_to_aws.png)](https://console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/new?stackName=efs-create-dashboard&templateURL=https://s3.amazonaws.com/aws-us-east-1/tutorial/create-efs-resources/efs-dashboard-with-size-monitor.yml) |


---
## Next section
### Click on the link below to go to the next Amazon EFS workshop section

| [**Create**](/workshop/1-create) |
| :---
---

For feedback, suggestions, or corrections, please email me at [darrylo@amazon.com](mailto:darrylo@amazon.com).
