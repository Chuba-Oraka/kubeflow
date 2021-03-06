---
title: "Clean up resources"
date: 2019-10-31T23:12:17-07:00
---

## Amazon EKS resources

#### Kill all distributed training jobs
```
kubectl delete MPIJobs --all
```

#### Delete all workflows and pipelines
```
kubectl delete workflow --all
```

#### Delete StorageClass, PersistentVolumeClaim and FSx for Lustre CSI Driver
{{% notice tip %}}
Note: This will automatically delete the FSx for luster file system. Your files are safe in Amazon S3.
{{% /notice %}}
```
kubectl delete -f specs/fsx-s3-sc.yaml
kubectl delete -f specs/fsx-s3-pvc.yaml
kubectl delete -f https://raw.githubusercontent.com/kubernetes-sigs/aws-fsx-csi-driver/master/deploy/kubernetes/manifest.yaml
```

#### Delete security group
```
aws ec2 delete-security-group --group-id ${SECURITY_GROUP_ID}
```

#### Delete policies attached to the instance role
These policies were automatically added to the node IAM roles, but we'll need to manually remove them.

* Copy the role associated with the worker instances
```
echo $INSTANCE_ROLE_NAME
```
* Navigate to IAM console
* Click on Roles on the left pane
* Search for the output of `echo $INSTANCE_ROLE_NAME`
* Delete the two inline policies.
 * `iam_alb_ingress_policy`
 * `iam_csi_fsx_policy`

#### Finally, delete the CPU cluster
```
eksctl delete cluster cpu
```
Optionally, delete the GPU cluster
```
eksctl delete cluster gpu
```

## SageMaker resources
SageMaker resources are easier to clear.
Login into the SageMaker console and click dashboard
Make sure that you don't have any resources that are **Green** as shown below. Click on the resources that is shown as green and either stop or delete them.

![sm_dashboard](/images/cleanup/sm_cleanup.png)

