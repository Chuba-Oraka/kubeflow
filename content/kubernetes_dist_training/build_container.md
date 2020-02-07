---
title: "Build training container image and push it to ECR"
date: 2019-10-28T16:51:02-07:00
weight: 9
---

#### Create a new Elastic Container Registry (ECR) repository
```
aws ecr create-repository --repository-name workshop
```


#### Build and push a custom Docker container
```
export ECR_REPOSITORY_URI=$(aws ecr describe-repositories --repository-names workshop --query "repositories[].repositoryUri" --output text)
echo "export ECR_REPOSITORY_URI=${ECR_REPOSITORY_URI}" | tee -a ~/.bash_profile
```


Log-in to the AWS Deep Learning registry.  We will extend the base Docker images in this repo.
```
$(aws ecr get-login --no-include-email --region us-west-2 --registry-ids 763104351884)

```
```
### EXPECTED OUTPUT ###
# ...
# Login Succeeded
```

In our Dockerfile we start with an AWS Deep Learning TensorFlow container and copy our training code into the container.

```
cd ~/SageMaker/aws-kubeflow-workshop/notebooks/part-3-kubernetes/docker/

cat Dockerfile.cpu

```
```
### EXPECTED OUTPUT ###
# FROM 763104351884.dkr.ecr.us-west-2.amazonaws.com/tensorflow-training:1.14.0-cpu-py36-ubuntu16.04
# COPY code /opt/training/
# WORKDIR /opt/training
```

Run `docker build` command
```
docker build -t ${ECR_REPOSITORY_URI}:latest -f Dockerfile.cpu . # <== MAKE SURE YOU INCLUDE THE `.` AT THE END!

```

Log in to your docker registry
```
$(aws ecr get-login --no-include-email --region us-west-2)

```
```
### EXPECTED OUTPUT ###
# ...
# Login Succeeded
```

Push your image to ECR

```
docker push ${ECR_REPOSITORY_URI}:latest

```

{{% notice tip %}}
What happened?
(1) You first logged into the AWS Deep Learning container registry in order to pull the deep learning container (2) You then built your container. (3) After the container is built, you added the appropriate tag needed to push it to ECR. (4) Then you login to your own registry. (4) Then you push the container to your registry

{{% /notice %}}
