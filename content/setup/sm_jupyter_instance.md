---
title: "Launch a SageMaker notebook instance"
date: 2019-10-27T22:39:43-07:00
weight: 4
---

{{% notice info %}}
**Note:** In this workshop, we'll be using an Amazon SageMaker notebook instance for simplicity and convenience.  SageMaker notebook instances have the correct privileges to access AWS services such as SageMaker, EKS, S3, ECR and others.  These notebooks also have pre-installed the AWS Command Line Interface (AWS CLI), python3, boto3 and SageMaker SDK installed. 
{{% /notice %}}

### Launch an Amazon SageMaker notebook instance

* Open the [AWS Management Console](https://console.aws.amazon.com/console/home)
{{% notice info %}}
**Note:** This workshop has been tested on the US West (Oregon) (us-west-2) region. Make sure that you see **Oregon** on the top right hand corner of your AWS Management Console. If you see a different region, click the dropdown menu and select US West (Oregon)
{{% /notice %}}

* In the AWS Console search bar, type `SageMaker` and select `Amazon SageMaker` to open the service console.

![SageMaker Console](/images/setup/setup_aws_console.png)

* Select `Create notebook instance`.
* In the Notebook instance name text box, enter a name for the notebook instance.
 * For this workshop select `workshop` as the instance name
 * Choose `ml.t3.xlarge`. We'll only be using this instance to launch jobs. The training job themselves will run either on a SageMaker managed cluster or an Amazon EKS cluster
 * Volume size `50` - this is only needed for building docker containers. During training data is copied directly from Amazon S3 to the training cluster when using SageMaker. When using Amazon EKS, we'll setup a distributed file system that worker nodes will use to get access to training data.

![Fill notebook instance](/images/setup/setup_fill_notebook.png)

* In the IAM role box, select the default `TeamRole`. 

* Keep the default settings for the other options and click `Create notebook instance`. On the `Notebook instances` section you should see the status change from `Pending` -> `InService`
* While the notebook instance spins up, continue to work on the next section, and we'll come back and launch the instance when it's ready.
