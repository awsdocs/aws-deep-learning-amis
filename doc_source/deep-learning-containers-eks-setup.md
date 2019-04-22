# Setup<a name="deep-learning-containers-eks-setup"></a>

This guide will help you setup a Deep Learning environment using Amazon Elastic Container Service for Kubernetes \(Amazon EKS\) and AWS Deep Learning Containers\. Using EKS you can scale a production\-ready environment for multi\-node training and inference with Kubernetes containers\.

If you're not familiar with Kubernetes or EKS yet, that's okay\. This guide and the related EKS documentation will help you understand how to use the family of Kubernetes tools so you can scale a deep learning training or inference system with Kubernetes and EKS\. This guide assumes that you're already familiar with your deep learning framework's multi\-node implementations, or for inference, how to set up an inference server outside of containers\.

A deep learning container setup on EKS consists of one or more containers, forming a cluster\. You can have dedicated cluster types, such as a cluster for training and a cluster for inference\. You might also want different instance types for your training or instance clusters depending on the demands of the deep learning neural networks and models you plan to deploy\.

**Topics**
+ [Custom Images](#deep-learning-containers-eks-setup-custom-images)
+ [Licensing](#deep-learning-containers-eks-setup-licensing)
+ [Security Config](#deep-learning-containers-eks-setup-security-config)
+ [Gateway Node](#deep-learning-containers-eks-setup-gateway-node)
+ [GPU Clusters](#deep-learning-containers-eks-setup-gpu-clusters)
+ [CPU Clusters](#deep-learning-containers-eks-setup-cpu-clusters)
+ [Test Your Clusters](#deep-learning-containers-eks-setup-test)
+ [Manage Your Clusters](#deep-learning-containers-eks-setup-manage)
+ [Cleanup](#deep-learning-containers-eks-setup-cleanup)

## Custom Images<a name="deep-learning-containers-eks-setup-custom-images"></a>

Custom images are helpful if you want to load your own code or datasets and have them available on each node in your cluster\. Examples are provided that use custom images, so you can try them out without needing to create your own when you get started\.
+ [AWS Deep Learning Containers Custom Images](deep-learning-containers-custom-images.md)

## Licensing<a name="deep-learning-containers-eks-setup-licensing"></a>

To use GPU hardware, you need to use an AMI that has the necessary GPU drivers\. We recommend using the EKS\-optimized AMI with GPU support, which you will use in subsequent steps of this guide\. Because this AMI includes third\-party software that requires an end user license agreement \(EULA\), you must subscribe to the EKS\-optimized AMI in AWS Marketplace and accept the EULA before you can use the AMI in your worker node groups\.

**Important**  
To subscribe to the AMI, visit the [AWS Marketplace](https://aws.amazon.com/marketplace/pp/B07GRHFXGM)\.

## Security Config<a name="deep-learning-containers-eks-setup-security-config"></a>

To use EKS you must have a user account that has access to several security permissions\. These are set with the IAM tool\.

1. Create an IAM user or update an existing IAM user\. Refer to the IAM documentation for further info on creating or editing an IAM user\.

1. Get the credentials of this user\. Under Users, select your user, select Security Credentials, select Create access key pair, and download the key pair or copy the info for use later\.

1. Add policies to this IAM user\. These will provide the required access for EKS, IAM, and EC2\. First, open the IAM console at https://console\.aws\.amazon\.com/iam/\.

1. Search for AmazonEKSAdminPolicy, click the checkbox\.

1. Search for AmazonCloudFormationPolicy, click the checkbox\.

1. Search for AmazonEC2FullAccess, click the checkbox\.

1. Search for IAMFullAccess , click the checkbox\.

1. Search AmazonEC2ContainerRegistryReadOnly, click the checkbox\.

1. Search AmazonEKS\_CNI\_Policy, click the checkbox\.

1. Search AmazonS3FullAccess, click the checkbox\.

1. Accept the changes\.

## Gateway Node<a name="deep-learning-containers-eks-setup-gateway-node"></a>

To setup an EKS cluster it is recommended to use the open source tool, `eksctl`\. It is also recommended to use an EC2 instance with the Deep Learning Base AMI \(Ubuntu\) as your base of operations for allocating and controlling your cluster\. It is possible to run these tools locally on your PC or an EC2 instance you already have running, however to simplify the guide we will assume you're using a Deep Learning Base AMI \(DLAMI\) with Ubuntu 16\.04\. We'll refer to this as your gateway node\. 

Before you start, consider the location of your training data or where you want to run your cluster for responding to inference requests\. Typically your data and cluster for training or inference should be in the same region\. Also, you will want to spin up your gateway node in this same region\. You can follow this quick [10 minute tutorial](https://aws.amazon.com/getting-started/tutorials/get-started-dlami/) that will guide you on how to launch a DLAMI to use as your gateway node\.

1. Login to your gateway node\.

1. Install or upgrade AWS CLI\. To access the required new kubernetes features, you must have the latest version\.

   ```
   $ sudo pip install --upgrade awscli
   ```

1. Install eksctl by running the following commands\. For further information on eksctl refer to: https://aws\.amazon\.com/blogs/opensource/eksctl\-eks\-cluster\-one\-command/

   ```
   $ curl --silent \
   --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" \
   | tar xz -C /tmp
   $ sudo mv /tmp/eksctl /usr/local/bin
   ```

1. Install `kubectl` by running the following commands\. For further information on kubeclt refer to: https://docs\.aws\.amazon\.com/eks/latest/userguide/install\-kubectl\.html

   ```
   $ curl -o kubectl https://amazon-eks.s3-us-west-2.amazonaws.com/1.11.5/2018-12-06/bin/linux/amd64/kubectl
   $ chmod +x ./kubectl
   $ mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH
   ```

1. Install `aws-iam-authenticator` by running the following commands\. For further information on aws\-iam\-authenticator refer to: https://docs\.aws\.amazon\.com/eks/latest/userguide/getting\-started\.html\#eks\-prereqs

   ```
   $ curl -o aws-iam-authenticator https://amazon-eks.s3-us-west-2.amazonaws.com/1.11.5/2018-12-06/bin/linux/amd64/aws-iam-authenticator
   $ chmod +x aws-iam-authenticator
   $ cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator && export PATH=$HOME/bin:$PATH
   ```

1. Run `aws configure` for the IAM user from the Security Configuration section\. You are copying the IAM user's AWS Access Key, then the AWS Secret Access Key that you accessed in the IAM console and pasting these into the prompts from `aws configure`\. 

1. Install ksonnet:

   ```
   $ export KS_VER=0.13.1
   $ export KS_PKG=ks_${KS_VER}_linux_amd64
   $ wget -O /tmp/${KS_PKG}.tar.gz https://github.com/ksonnet/ksonnet/releases/download/v${KS_VER}/${KS_PKG}.tar.gz
   $ mkdir -p ${HOME}/bin
   $ tar -xvf /tmp/$KS_PKG.tar.gz -C ${HOME}/bin
   $ sudo mv ${HOME}/bin/$KS_PKG/ks /usr/local/bin
   ```

## GPU Clusters<a name="deep-learning-containers-eks-setup-gpu-clusters"></a>

1. Examine the following command to create a cluster using a p3\.8xlarge instance type\. You will need to make modifications to it before you run it\.
   + name is what you will use to manage your cluster\. You can change `cluster-name` to be whatever name you like as long as there are no spaces or special characters\.
   + nodes is the number of containers you want in your cluster\. In this example, we're starting with three nodes\.
   + node\-type refers to instance class\. You can choose a different instance class if you already know what kind will work best for your situation\. 
   + timeout and \*ssh\-access \*can be left alone\.
   + ssh\-public\-key is the name of the key that you want to use to login your worker nodes\. Either use a security key you already use or create a new one but be sure to swap out the ssh\-public\-key with a key that was allocated for the region you used\. Note: You only need to provide the key name as seen in the 'key pairs' section of the EC2 Console\. 
   + region is the EC2 region where the cluster will be launched\. If you plan to use training data that resides in a specific region \(other than *<us\-east\-1>*\) it is recommended that you use the same region\. The ssh\-public\-key must have access to launch instances in this region\.
**Note**  
The rest of this guide assumes *<us\-east\-1>* as the region\.
   + auto\-kubeconfig can be left alone\.

1. Once you have made changes to the command, run it, and wait\. It can take several minutes for a single node cluster, and will take even longer if you chose to create a large cluster\.

   ```
   $ eksctl create cluster <cluster-name> \
                         --version 1.11 \
                         --nodes 3 \
                         --node-type=<p3.8xlarge> \
                         --timeout=40m \
                         --ssh-access \
                         --ssh-public-key <key_pair_name> \
                         --region <us-east-1> \
                         --auto-kubeconfig
   ```

   You should see something similar to the following output:

   ```
   EKS cluster "training-1" in "us-east-1" region is ready
   ```

1. Ideally the auto\-kubeconfig should have configured your cluster\. However, if you run into issues you can run the command below to set your kubeconfig\. This command can also be used if you want to change your gateway node and manage your cluster from elsewhere\.

   ```
   $ aws eks --region <region> update-kubeconfig --name <cluster-name>
   ```

   You should see something similar to the following output:

   ```
   Added new context arn:aws:eks:us-east-1:841569659894:cluster/training-1 to /home/ubuntu/.kube/config
   ```

1. If you plan to use GPU instance types, make sure to run the following step to install the NVIDIA device plugin for Kubernetes:

   ```
   $ kubectl apply -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v1.11/nvidia-device-plugin.yml
   ```

1. Verify the GPUs available on each node in your cluster

   ```
   $ kubectl get nodes "-o=custom-columns=NAME:.metadata.name,GPU:.status.allocatable.nvidia\.com/gpu"
   ```

## CPU Clusters<a name="deep-learning-containers-eks-setup-cpu-clusters"></a>

Refer to the previous section's discussion on using the eksctl command to launch a GPU cluster, and modify `node-type` to use a CPU instance type\.

## Test Your Clusters<a name="deep-learning-containers-eks-setup-test"></a>

1. You can run a kubectl command on the cluster to check its status\. Try the command to make sure it is picking up the current cluster you want to manage\. 

   ```
   $ kubectl get nodes -o wide
   ```

1. Take a look in \~/\.kube\. This directory has the kubeconfig files for the various clusters configured from your gateway node\. If you browse further into the folder you can find \~/\.kube/eksctl/clusters \- This holds the kubeconfig file for clusters created using eksctl\. This file has some details which you ideally shouldn't have to modify, since the tools are generating and updating the configs for you, but it is good to reference when troubleshooting\. 

1. Verify that the cluster is active\.

   ```
   $ aws eks --region <region> describe-cluster --name <cluster-name> --query cluster.status
   ```

   You should see the following output:

   ```
   "ACTIVE"
   ```

1. Verify the kubectl context if you have multiple clusters set up from the same host instance\. Sometimes it helps to make sure that the default context found by kubectl is set properly\. Check this using the following command: 

   ```
   $ kubectl config get-contexts
   ```

1. If the context is not set as expected, fix this using the following command: 

   ```
   $ aws eks --region <region> update-kubeconfig --name <cluster-name>
   ```

## Manage Your Clusters<a name="deep-learning-containers-eks-setup-manage"></a>
+ When you want to control or query a cluster you can address it by the config file using the kubeconfig parameter\. This is useful when you have more than one cluster\. For example, if you have a separate cluster called “training\-gpu\-1” you can call the get pods command on it by passed the config file as a parameter as follows:

  ```
  $ kubectl --kubeconfig=/home/ubuntu/.kube/eksctl/clusters/training-gpu-1 get pods
  ```
+ It is useful to note that you can run this same command without the kubeconfig parameter and it will report status on your current actively controlled cluster\.

  ```
  $ kubectl get pods
  ```
+ If you setup multiple clusters and they have yet to have the NVIDIA plugin installed, you can install it this way:

  ```
  $ kubectl --kubeconfig=/home/ubuntu/.kube/eksctl/clusters/training-gpu-1 apply -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v1.10/nvidia-device-plugin.yml
  ```
+ You also change the active cluster by updating the kubeconfig, passing the name of the cluster you want to manage\. The following command updates kubeconfig and removes the need to use the kubeconfig parameter\.

  ```
  $ aws eks —region us-east-1 update-kubeconfig —name training-gpu-1
  ```
+ Once you have updated the config the previous example of installing the NVIDIA plugin is accomplished as follows:

  ```
  $ kubectl apply -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v1.11/nvidia-device-plugin.yml
  ```
+ If you follow all of the examples in this guide you will frequently switch between active clusters, so you can orchestrate training or inference or use different frameworks running on different clusters\.

## Cleanup<a name="deep-learning-containers-eks-setup-cleanup"></a>

When you're done using the cluster you can delete it to avoid incurring additional costs\.

```
$ eksctl delete cluster --name=<cluster-name>
```

If you only want to delete a pod run the following:

```
$ kubectl delete pods <name>
```

If you want to delete a ksonnet job, change directories to where you launched the job and run the following:

```
$ ks delete default
$ ks component rm openmpi
```

If you want to reset the secret for access to the cluster run the following:

```
$ kubectl delete secret ${SECRET} -n ${NAMESPACE} || true
```

If you want to delete a nodegroup attached to a cluster, run the following:

```
$ eksctl delete nodegroup --name <cluster_name>
```

To attach a nodegroup to a cluster, run the following:

```
$ eksctl create nodegroup
                      --cluster <cluster-name> \
                      --node-ami <ami_id> \
                      --nodes <num_nodes> \
                      --node-type=<instance_type> \
                      --timeout=40m \
                      --ssh-access \
                      --ssh-public-key <key_pair_name> \
                      --region <us-east-1> \
                      --auto-kubeconfig
```