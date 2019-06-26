# Distributed GPU Training<a name="deep-learning-containers-eks-tutorials-distributed-gpu-training"></a>

This section is for running distributed training on multi\-node GPU clusters\.

For a complete list of AWS Deep Learning Containers, refer to [Deep Learning Containers Images](deep-learning-containers-images.md)\. 

**Topics**
+ [MXNet](#deep-learning-containers-eks-tutorials-distributed-gpu-training-mxnet)
+ [TensorFlow with Horovod](#deep-learning-containers-eks-tutorials-distributed-gpu-training-tf)

To run distributed training on EKS, you will be using Kubeflow\. The Kubeflow project is dedicated to making deployments of machine learning \(ML\) workflows on Kubernetes simple, portable and scalable\. 

# Setup your cluster for distributed training by installing Kubeflow

1. Install Kubeflow\.

   ```
   $ export KUBEFLOW_VERSION=0.4.1
   $ curl https://raw.githubusercontent.com/kubeflow/kubeflow/v${KUBEFLOW_VERSION}/scripts/download.sh | bash
   ```

1. When using Kubeflow packages, you will soon run into Github API limits\. You need to create a [Github token](https://github.com/settings/tokens) and export it as follows\. You do not need to select any scopes\.

   ```
   $ export GITHUB_TOKEN=<token>
   ```

## MXNet<a name="deep-learning-containers-eks-tutorials-distributed-gpu-training-mxnet"></a>

This tutorial will guide you on distributed training with MXNet on your multi\-node GPU cluster\. It uses Parameter Server\. To run MXNet distributed training on EKS, we will use the [Kubernetes MXNet\-operator](https://www.kubeflow.org/docs/components/mxnet/) called `MXJob`\. It provides a custom resource that makes it easy to run distributed or non\-distributed MXNet jobs \(training and tuning\) on Kubernetes\.

## Add MXNet support in your Kubeflow deployment

1. Setup a namespace\.

   ```
   $ NAMESPACE=kubeflow-dist-train-mx; kubectl --kubeconfig=/home/ubuntu/.kube/eksctl/clusters/training-gpu-1 create namespace ${NAMESPACE}
   ```

1. Set the app name and initialize it\.

   ```
   $ APP_NAME=kubeflow-mx-ps; ks init ${APP_NAME}; cd ${APP_NAME}
   ```

1. Change the namespace used by the default environment to `${NAMESPACE}`\.

   ```
   $ ks env set default --namespace ${NAMESPACE}
   ```

1. Install MXNet\-operator for kubeflow\. This is needed to run MXNet distributed training with parameter server\.

   ```
   $ ks registry add kubeflow github.com/kubeflow/kubeflow/tree/${KUBEFLOW_VERSION}/kubeflow
   $ ks pkg install kubeflow/mxnet-job
   ```

1. Generate Kubernetes\-compatible, jsonnet component manifest file\.

   ```
   $ ks generate mxnet-operator mxnet-operator
   ```

1. Apply the configuration settings\.

   ```
   $ ks apply default -c mxnet-operator
   ```

1. Using a Custom Resource Definition \(CRD\) gives users the ability to create and manage MX Jobs just like builtin K8s resources\. Verify that the MXNet custom resource is installed\.

   ```
   $ kubectl get crd
   ```

   The output should include `mxjobs.kubeflow.org`\.

## Running MNIST distributed training with parameter server example

Your first task is to create a pod file\(mx\_job\_dist\.yaml\) for your job according to the available cluster configuration and job to run\. There are 3 jobModes you need to specify: Scheduler, Server and Worker\. You can specify how many pods you want to spawn with the field replicas\. 
+ Scheduler: There is only one scheduler\. The role of the scheduler is to set up the cluster\. This includes waiting for messages that each node has come up and which port the node is listening on\. The scheduler then lets all processes know about every other node in the cluster, so that they can communicate with each other\.
+ Server: There can be multiple servers which store the model’s parameters, and communicate with workers\. A server may or may not be co\-located with the worker processes\.
+ Worker: A worker node actually performs training on a batch of training samples\. Before processing each batch, the workers pull weights from servers\. The workers also send gradients to the servers after each batch\. Depending on the workload for training a model, it might not be a good idea to run multiple worker processes on the same machine\.
+ Provide container image you want to use with the field image\.
+ You can provide `restartPolicy` from one of the Always, OnFailure and Never\. It determines whether pods will be restarted when they exit or not\.
+ Provide container image you want to use with the field image\.

1. Based on the previous discussion, you would modify the following code block according to your requirements and save it in a file called `mx_job_dist.yaml`\.

   ```
   apiVersion: "kubeflow.org/v1alpha1"
   kind: "MXJob"
   metadata:
     name: "gpu-dist-job"
   spec:
     jobMode: "dist"
     replicaSpecs:
       - replicas: 1
         mxReplicaType: SCHEDULER
         PsRootPort: 9000
         template:
           spec:
             containers:
               - image: 763104351884.dkr.ecr.us-east-1.amazonaws.com/aws-samples-mxnet-training:1.4.1-gpu-py36-cu100-ubuntu16.04-example
                 name: mxnet
             restartPolicy: OnFailure
       - replicas: 2
         mxReplicaType: SERVER
         template:
           spec:
             containers:
               - image: 763104351884.dkr.ecr.us-east-1.amazonaws.com/aws-samples-mxnet-training:1.4.1-gpu-py36-cu100-ubuntu16.04-example
                 name: mxnet
             restartPolicy: OnFailure
       - replicas: 2
         mxReplicaType: WORKER
         template:
           spec:
             containers:
               - image: 763104351884.dkr.ecr.us-east-1.amazonaws.com/aws-samples-mxnet-training:1.4.1-gpu-py36-cu100-ubuntu16.04-example
                 name: mxnet
                 command: ["python"]
                 args: ["/incubator-mxnet/example/image-classification/train_mnist.py","--num-epochs","1","--num-layers","2","--kv-store","dist_device_sync","--gpus","0,1"]
                 resources:
                   limits:
                     nvidia.com/gpu: 2
             restartPolicy: OnFailure
   ```

1. Run distributed training job with the pod file you just created\.

   ```
   $ # Create a job by defining MXJob
   kubectl create -f mx_job_dist.yaml
   ```

1. List the running jobs\.

   ```
   $ kubectl get mxjobs
   ```

1. To get status of a running job, run the following\. Replace the JOB variable with whatever the job's name is\.

   ```
   $ JOB=gpu-dist-job
   kubectl get -o yaml mxjobs $JOB
   ```

   The output should be similar to the following:

   ```
   apiVersion: kubeflow.org/v1alpha1
   kind: MXJob
   metadata:
     creationTimestamp: 2019-03-21T22:00:38Z
     generation: 1
     name: gpu-dist-job
     namespace: default
     resourceVersion: "2523104"
     selfLink: /apis/kubeflow.org/v1alpha1/namespaces/default/mxjobs/gpu-dist-job
     uid: c2e67f05-4c24-11e9-a6d4-125f5bb10ada
   spec:
     RuntimeId: j1ht
     jobMode: dist
     mxImage: jzp1025/mxnet:test
     replicaSpecs:
     - PsRootPort: 9000
       mxReplicaType: SCHEDULER
       replicas: 1
       template:
         metadata:
           creationTimestamp: null
         spec:
           containers:
           - image: 763104351884.dkr.ecr.us-east-1.amazonaws.com/aws-samples-mxnet-training:1.4.1-gpu-py36-cu100-ubuntu16.04-example
             name: mxnet
             resources: {}
           restartPolicy: OnFailure
     - PsRootPort: 9091
       mxReplicaType: SERVER
       replicas: 2
       template:
         metadata:
           creationTimestamp: null
         spec:
           containers:
           - image: 763104351884.dkr.ecr.us-east-1.amazonaws.com/aws-samples-mxnet-training:1.4.1-gpu-py36-cu100-ubuntu16.04-example
             name: mxnet
             resources: {}
     - PsRootPort: 9091
       mxReplicaType: WORKER
       replicas: 2
       template:
         metadata:
           creationTimestamp: null
         spec:
           containers:
           - args:
             - /incubator-mxnet/example/image-classification/train_mnist.py
             - --num-epochs
             - "15"
             - --num-layers
             - "2"
             - --kv-store
             - dist_device_sync
             - --gpus
             - "0"
             command:
             - python
             image: 763104351884.dkr.ecr.us-east-1.amazonaws.com/aws-samples-mxnet-training:1.4.1-gpu-py36-cu100-ubuntu16.04-example
             name: mxnet
             resources:
               limits:
                 nvidia.com/gpu: 1
           restartPolicy: OnFailure
     terminationPolicy:
       chief:
         replicaIndex: 0
         replicaName: WORKER
   status:
     phase: Running
     reason: ""
     replicaStatuses:
     - ReplicasStates:
         Running: 1
       mx_replica_type: SCHEDULER
       state: Running
     - ReplicasStates:
         Running: 2
       mx_replica_type: SERVER
       state: Running
     - ReplicasStates:
         Running: 2
       mx_replica_type: WORKER
       state: Running
     state: Running
   ```
**Note**  
Status provides information about the state of the resources\.  
Phase \- Indicates the phase of a job and will be one of Creating, Running, CleanUp, Failed, Done\.  
State \- Provides the overall status of the job and will be one of Running, Succeeded, Failed\.

1. To cleanup and rerun a job:

   ```
   $ eksctl delete cluster --name=<cluster-name>
   ```

   If you want to delete a job, change directories to where you launched the job and run the following:

   ```
   $ ks delete default
   $ kubectl delete -f mx_job_dist.yaml
   ```

## TensorFlow with Horovod<a name="deep-learning-containers-eks-tutorials-distributed-gpu-training-tf"></a>

This tutorial will guide you on how to setup distributed training of TensorFlow models on your multi\-node GPU cluster\. It also uses Horovod\. It uses an example image that already has a training script included, and it uses a 3\-node cluster with `node-type=p3.16xlarge`\.

1. Set the app name and initialize it\.

   ```
   $ APP_NAME=kubeflow-tf-hvd; ks init ${APP_NAME}; cd ${APP_NAME}
   ```

1. Install mpi\-operator from kubeflow in this app's folder\.

   ```
   $ KUBEFLOW_VERSION=master
   $ ks registry add kubeflow github.com/kubeflow/kubeflow/tree/${KUBEFLOW_VERSION}/kubeflow
   $ ks pkg install kubeflow/common@${KUBEFLOW_VERSION}
   $ ks pkg install kubeflow/mpi-job@${KUBEFLOW_VERSION}
   $ ks generate mpi-operator mpi-operator
   $ ks apply default -c mpi-operator
   ```

1. Create a MPI Job template and define the number of nodes \(replicas\) and number of GPUs each node has \(gpusPerReplica\), you can also bring your image and customize command\. 

   ```
   IMAGE="763104351884.dkr.ecr.us-east-1.amazonaws.com/aws-samples-tensorflow-training:1.13-horovod-gpu-py36-cu100-ubuntu16.04-example"
   GPUS_PER_WORKER=2
   NUMBER_OF_WORKERS=3
   JOB_NAME=tf-resnet50-horovod-job
   ks generate mpi-job-custom ${JOB_NAME}
   
   ks param set ${JOB_NAME} replicas ${NUMBER_OF_WORKERS}
   ks param set ${JOB_NAME} gpusPerReplica ${GPUS_PER_WORKER}
   ks param set ${JOB_NAME} image ${IMAGE}
   
   ks param set ${JOB_NAME} command "mpirun,-mca,btl_tcp_if_exclude,lo,-mca,pml,ob1,-mca,btl,^openib,--bind-to,none,-map-by,slot,-x,LD_LIBRARY_PATH,-x,PATH,-x,NCCL_SOCKET_IFNAME=eth0,-x,NCCL_DEBUG=INFO,python,/deep-learning-models/models/resnet/tensorflow/train_imagenet_resnet_hvd.py"
   ks param set ${JOB_NAME} args -- --num_epochs=10,--synthetic
   ```

1. Check the job manifest that was created to verify everything looks okay\.

   ```
   $ ks show default -c ${JOB_NAME}
   ```

1. Now apply the manifest to the default environment\. The MPI Job will create a launch pod and logs will be aggregated in this pod\.

   ```
   $ ks apply default -c ${JOB_NAME}
   ```

1. Check the status\. The name of the job “tensorflow\-training” was in the tf\.yaml file\. It will now appear in the status\. If you're running any other tests or have previously run something, it will appear in this list\. Run this several times until you see the status change to “Running”\.

   ```
   $ kubectl get pods -o wide
   ```

   You should see something similar to the following output:

   ```
   NAME                                     READY     STATUS    RESTARTS   AGE       IP               NODE                             NOMINATED NODE
   mpi-operator-5fff9d76f5-wvf56            1/1       Running   0          23m       192.168.10.117   ip-192-168-22-21.ec2.internal    <none>
   tf-resnet50-horovod-job-launcher-d7w6t   1/1       Running   0          21m       192.168.13.210   ip-192-168-4-253.ec2.internal    <none>
   tf-resnet50-horovod-job-worker-0         1/1       Running   0          22m       192.168.17.216   ip-192-168-4-253.ec2.internal    <none>
   tf-resnet50-horovod-job-worker-1         1/1       Running   0          22m       192.168.20.228   ip-192-168-27-148.ec2.internal   <none>
   tf-resnet50-horovod-job-worker-2         1/1       Running   0          22m       192.168.11.70    ip-192-168-22-21.ec2.internal    <none>
   ```

1. Based on the name of the launcher pod above, check the logs to see the training output\.

   ```
   $ kubectl logs -f --tail 10 tf-resnet50-horovod-job-launcher-d7w6t
   ```

1. You can check the logs to watch the training progress\. You can also continue to check “get pods” to refresh the status\. When the status changes to “Completed” you will know that the training job is done\.

1. To cleanup and rerun a job:

   ```
   # make sure ${JOB_NAME} and ${APP_NAME} are still set
   $ ks delete default -c ${JOB_NAME}
   $ ks delete default
   $ cd .. && rm -rf ${APP_NAME}
   ```