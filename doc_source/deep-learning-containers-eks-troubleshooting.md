# Troubleshooting AWS Deep Learning Containers on EKS<a name="deep-learning-containers-eks-troubleshooting"></a>

## Troubleshooting<a name="dWE9CAitw9Q"></a>

**Topics**
+ [Setup Errors](#dWE9CAJejUV)
+ [Usage Errors](#dWE9CAFNALa)
+ [Cleanup Errors](#dWE9CAeZJq5)

### Setup Errors<a name="dWE9CAJejUV"></a>

 Error: ERROR registry `kubeflow` does not exist 

 Example: 

```
$ ks pkg install kubeflow/tf-serving
ERROR registry 'kubeflow' does not exist
```

 Solution: Run the following command\.  

```
ks registry add kubeflow github.com/google/kubeflow/tree/master/kubeflow
```

 Error: context deadline exceeded 

 Example: 

```
$ eksctl create cluster <args>
[✖] waiting for CloudFormation stack "eksctl-training-cluster-1-nodegroup-ng-8c4c94bc" to reach "CREATE_COMPLETE" status: RequestCanceled: waiter context canceled
caused by: context deadline exceeded
```

 Solution: Verify that you have not exceeded capacity for your account\. Retry the command in a different region\.  

```
$ kubectl get nodes
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```

 Solution: Try running `cp ~/.kube/eksctl/clusters/<name-of-cluster> ~/.kube/config` 

```
$ ks apply default
ERROR handle object: patching object from cluster: merging object with existing state: Unauthorized
```

Solution: This is a concurrency issue that can occur when multiple users with different authorization/credentials try to start jobs on the same cluster\.* *

```
$ APP_NAME=kubeflow-tf-hvd; ks init ${APP_NAME}; cd ${APP_NAME}
INFO Using context "arn:aws:eks:eu-west-1:999999999999:cluster/training-gpu-1" from kubeconfig file "/home/ubuntu/.kube/config"
ERROR Could not create app; directory '/home/ubuntu/kubeflow-tf-hvd' already exists
```

 Solution: ignore the warning\. However you may have additional cleanup to do inside that folder\. You may want to delete the folder to simplify cleanup\. 

### Usage Errors<a name="dWE9CAFNALa"></a>

```
ssh: Could not resolve hostname openmpi-worker-1.openmpi.kubeflow-dist-train-tf: Name or service not known
```

 Solution: At any point while using the EKS cluster, if you see this error message, run the NVIDIA device plugin for Kubernetes installation step again\. Make sure that you have targeted the right cluster by either passing in the specific config file or by switching your active cluster to the targeted cluster\. 

### Cleanup Errors<a name="dWE9CAeZJq5"></a>

```
$ kubectl delete namespace ${NAMESPACE}
error: the server doesn't have a resource type "namspace"
```

 Solution: check the spelling of your namespace\. It might be a typo\. 

```
$ ks delete default
ERROR the server has asked for the client to provide credentials
```

 Solution: Make sure that `~/.kube/config` points to the correct cluster and that AWS credentials have been correctly configured using `aws configure` or by exporting AWS environment variables 

```
$ ks delete default
ERROR finding app root from starting path: : unable to find ksonnet project
$ kubectl logs -n ${NAMESPACE} -f ${COMPONENT}-master > results/benchmark_1.out
Error from server (NotFound): pods "openmpi-master" not found
```

 Solution: Make sure you are cd'ed correctly into the ksonnet app created \(the folder where `ks init` was executed\), and note that deleting the default context will cause the corresponding resources to be deleted\. 

```
$ ks component rm openmpi
ERROR finding app root from starting path: : unable to find ksonnet project
```

 Solution: Make sure you are cd'ed correctly into the ksonnet app created \(the folder where `ks init` was executed\)\. 