---
title: "Prepare training dataset"
date: 2019-10-28T01:48:16-07:00
weight: 2
---
### Download the CIFAR10 dataset and upload it to Amazon S3

In your Jupyter terminal window, run the following commands:

```
cd ~/SageMaker/kubeflow/notebooks/

# Activate the TensorFlow conda environment
source activate tensorflow_p36

# Download CIFAR10 dataset and convert it to TFRecords format
python generate_cifar10_tfrecords.py --data-dir dataset

export S3_BUCKET=sagemaker-$(aws sts get-caller-identity | jq -r '.Account')-$(aws configure get region)
echo "export S3_BUCKET=${S3_BUCKET}" | tee -a ~/.bash_profile

# Create a new S3 bucket and upload the dataset to it. 
aws s3 mb s3://${S3_BUCKET}

aws s3 sync dataset/ s3://${S3_BUCKET}/cifar10-dataset/

echo "Completed"

```
