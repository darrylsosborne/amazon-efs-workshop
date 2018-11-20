![](https://s3.amazonaws.com/aws-us-east-1/tutorial/AWS_logo_PMS_300x180.png)

![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_available.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ingergration.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_ecryption-lock.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_fully-managed.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_lowcost-affordable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_performance.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_scalable.png)![](https://s3.amazonaws.com/aws-us-east-1/tutorial/100x100_benefit_storage.png)

# **Amazon Elastic File System (Amazon EFS)**

## Workshop

### Version 2018.02

efs.wrkshp.2018.02

---

© 2018 Amazon Web Services, Inc. and its affiliates. All rights reserved. This work may not be  reproduced or redistributed, in whole or in part, without prior written permission from Amazon Web Services, Inc. Commercial copying, lending, or selling is prohibited.

Errors or corrections? Email us at [darrylo@amazon.com](mailto:darrylo@amazon.com).

---

### Table of Contents  
[Workshops](#workshops) 

[0. Pre-requisites](#0-pre-requisites)

[1. Create a file system](#1-create-a-file-system)

[2. Performance](#2-performance) 

[3. Scale-out](#3-scale-out)

[4. Monitoring](#3-monitoring)

---

### Workshop

This workshop designed to help you better understand the performance characteristics of Amazon Elastic File System (Amazon EFS) and how parallelism, I/O size, and Amazon EC2 instance types affects file system IOPS and throughput. You will also gain an understanding of the different performance and throughput modes a file system can be using.
#
### 0. Pre-requisites [optional]
This section is an AWS Cloudformation template that will create two Amazon VPCs with Internet Gateways, Security Groups, and routing tables to create isolate networks for this workshop. It is highly recommended to use setup these pre-requisites. If you decide to use your own VPCs, make sure they allow SSH access from your laptop.

Click on the link below to go to the **Pre-requisites** section. Once you've finished that section, move on to **Create a file system**.
 
| :---
| [**Pre-requisites**](/workshop/pre-requisites)


### 1. Create a file system
This section is a set of AWS Cloudformation templates that will create an Amazon EFS file system and pre-load data to grow the file system to obtain higher levels of IOPS and throughput. Throughput and IOPS on Amazon EFS scales as a file system grows, so larger file systems are able to achieve higher levels of throughput and IOPS. Because file-based workloads are typically spiky—driving high levels of throughput for short periods of time, and low levels of throughput the rest of the time—Amazon EFS is designed to burst to high throughput levels for periods of time. Amazon EFS uses a credit system to determine when file systems can burst. File systems can be monitored using AWS CloudWatch metrics. These Cloudformation templates will also create an AWS CloudWatch dashboard, custom metrics, alarms, scheduled events, AWS Lambda function, SNS notification, and an Auto Scaling group to monitor and dynamically adjust alarm thresholds as the file system grows and shrinks.

Click on the link below to go to the **Create a file system** workshop. Once you've finished that workshop move on to **Performance**.

| Workshop 
| ---: | ---
| **Create a file system** | [![](/images/efs_workshop.png)](/workshop/create-file-system) |


### 2. Performance
This section is a set of scripts that will demonstrate:
- different instance types provide different levels of network performance when accessing a file system
- different I/O sizes (block sizes) and sync() freqencies (the rate data is persisted to disk) effects file system throughput
- increasing the number of threads accessing a file system will increase IOPS and throughput

Click on the ![](/images/efs_workshop.png) link below to go to the **Performance** workshop. Once you've finished that workshop move on to **Scale-out**.

| Workshop | Link
| --- | ---
| **Performance** | [![](/images/efs_workshop.png)](/workshop/performance) |


### 3. Scale-out
This section is a Cloudformation template that will create an Amazon EC2 spot fleet and download objects in parallel from an Amazon S3 bucket.

Click on the ![](/images/efs_workshop.png) link below to go to the **Scale-out** workshop. Once you've finished that workshop move on to the **Scenarios**.

| Workshop | Link
| --- | ---
| **Scale-out** | [![](/images/efs_workshop.png)](/workshop/scale-out) |


### 4. Monitoring
This section is a guide to setup an AWS CloudWatch dashboard with widgets to help monitor how your workload is driving an Amazon EFS file system.

Click on the ![](/images/efs_tutorial.png) link below to go to the **Monitoring** tutorial. 

| Workshop | Link
| --- | ---
| **Monitoring** | [![](/images/efs_tutorial.png)](/tutorial/monitoring) |


---

## Troubleshooting


For feedback, suggestions, or corrections, please email me at [darrylo@amazon.com](mailto:darrylo@amazon.com).


## License

This library is licensed under the Amazon Software License.
