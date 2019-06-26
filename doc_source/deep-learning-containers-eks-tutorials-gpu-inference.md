# GPU Inference<a name="deep-learning-containers-eks-tutorials-gpu-inference"></a>

This section will guide you on how to run inference on AWS Deep Learning Containers for EKS GPU clusters using MXNet and TensorFlow\.

For a complete list of AWS Deep Learning Containers, refer to [Deep Learning Containers Images](deep-learning-containers-images.md)\. 

**Note**  
MKL users: read the [AWS Deep Learning Containers MKL Recommendations](deep-learning-containers-mkl.md) to get the best training or inference performance\.

**Topics**
+ [MXNet](#deep-learning-containers-eks-tutorials-gpu-inference-mxnet)
+ [TensorFlow](#deep-learning-containers-eks-tutorials-gpu-inference-tf)

## MXNet<a name="deep-learning-containers-eks-tutorials-gpu-inference-mxnet"></a>

In this approach, you create a Kubernetes Service and a Deployment\. A service exposes a process and its ports, and Deployment, among its other features, responsible for ensuring that a certain number of pods \(in the following case, at least one\) is always up and running\.

1. For GPU\-base inference, install the NVIDIA device plugin for Kubernetes:

   ```
   $ kubectl apply -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v1.11/nvidia-device-plugin.yml
   ```

1. Verify that the nvidia\-device\-plugin\-daemonset is running correctly\.

   ```
   $ kubectl get daemonset -n kube-system
   ```

   The output will be similar to the following:

   ```
   NAME                             DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
   aws-node                         3         3         3         3            3           <none>          6d
   kube-proxy                       3         3         3         3            3           <none>          6d
   nvidia-device-plugin-daemonset   3         3         3         3            3           <none>          57s
   ```

1. Create the namespace\. You might need to change the kubeconfig to point to the right cluster\. Verify that you have setup a "training\-gpu\-1" or change this to your GPU cluster's config\.

   ```
   $ NAMESPACE=mx-inference; kubectl —kubeconfig=/home/ubuntu/.kube/eksctl/clusters/training-gpu-1 create namespace ${NAMESPACE}
   ```

1. \(Optional step when using public models\.\) Setup your model at a network location that is mountable e\.g\., in S3\. Refer to the steps to upload a trained model to S3 mentioned in the section Inference with TensorFlow\. Do not forget to apply the secret to your namespace:

   ```
   $ kubectl -n ${NAMESPACE} apply -f secret.yaml
   ```

1. Create the file `mx_inference.yaml`\. Use the contents of the next code block as its content\.

   ```
   ---
   kind: Service
   apiVersion: v1
   metadata:
     name: squeezenet-service
     labels:
       app: squeezenet-service
   spec:
     ports:
     - port: 8080
       targetPort: mms
     selector:
       app: squeezenet-service
   ---
   kind: Deployment
   apiVersion: apps/v1
   metadata:
     name: squeezenet-service
     labels:
       app: squeezenet-service
   spec:
     replicas: 1
     selector:
       matchLabels:
         app: squeezenet-service
     template:
       metadata:
         labels:
           app: squeezenet-service
       spec:
         containers:
         - name: squeezenet-service
           image: 763104351884.dkr.ecr.us-east-1.amazonaws.com/mxnet-inference:1.4.2-gpu-py36-cu100-ubuntu16.04
           args:
           - mxnet-model-server
           - --start
           - --mms-config /home/model-server/config.properties
           - --models squeezenet=https://s3.amazonaws.com/model-server/model_archive_1.0/squeezenet_v1.1.mar
           ports:
           - name: mms
             containerPort: 8080
           - name: mms-management
             containerPort: 8081
           imagePullPolicy: IfNotPresent
           resources:
             limits:
               cpu: 4
               memory: 4Gi
               nvidia.com/gpu: 1
             requests:
               cpu: "1"
               memory: 1Gi
   ```

1. Apply the configuration to a new pod in the previously defined namespace:

   ```
   $ kubectl -n ${NAMESPACE} apply -f mx_inference.yaml
   ```

   Your output should be similar to the following:

   ```
   service/squeezenet-service created
   deployment.apps/squeezenet-service created
   ```

1. Check status of the pod and wait for the pod to be in “RUNNING” state:

   ```
   $ kubectl get pods -n ${NAMESPACE}
   ```

1. Repeat the check status step until you see the following "RUNNING" state:

   ```
   NAME                     READY     STATUS    RESTARTS   AGE
   squeezenet-service-xvw1  1/1       Running   0          3m
   ```

1. To further describe the pod, you can run:

   ```
   $ kubectl describe pod <pod_name> -n ${NAMESPACE}
   ```

1. Since the serviceType here is ClusterIP, you can forward the port from your container to your host machine \(the ampersand runs this in the background\):

   ```
   $ kubectl port-forward -n ${NAMESPACE} `kubectl get pods -n ${NAMESPACE} --selector=app=squeezenet-service -o jsonpath='{.items[0].metadata.name}'` 8080:8080 &
   ```

1. Download an image of a kitten:

   ```
   $ curl -O https://s3.amazonaws.com/model-server/inputs/kitten.jpg
   ```

1. Run inference on the model:

   ```
   $ curl -X POST http://127.0.0.1:8080/predictions/squeezenet -T kitten.jpg
   ```

## TensorFlow<a name="deep-learning-containers-eks-tutorials-gpu-inference-tf"></a>

In this approach, you create a Kubernetes Service and a Deployment\. A service exposes a process and its ports, and Deployment, among its other features, responsible for ensuring that a certain number of pods \(in the following case, at least one\) is always up and running\.

1. For GPU\-base inference, install the NVIDIA device plugin for Kubernetes:

   ```
   $ kubectl apply -f https://raw.githubusercontent.com/NVIDIA/k8s-device-plugin/v1.11/nvidia-device-plugin.yml
   ```

1. Verify that the nvidia\-device\-plugin\-daemonset is running correctly\.

   ```
   $ kubectl get daemonset -n kube-system
   ```

   The output will be similar to the following:

   ```
   NAME                             DESIRED   CURRENT   READY     UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
   aws-node                         3         3         3         3            3           <none>          6d
   kube-proxy                       3         3         3         3            3           <none>          6d
   nvidia-device-plugin-daemonset   3         3         3         3            3           <none>          57s
   ```

1. Create the namespace\. You might need to change the kubeconfig to point to the right cluster\. Verify that you have setup a "training\-gpu\-1" or change this to your GPU cluster's config:

   ```
   $ NAMESPACE=mx-inference; kubectl —kubeconfig=/home/ubuntu/.kube/eksctl/clusters/training-gpu-1 create namespace ${NAMESPACE}
   ```

1. Models served for inference can be retrieved in different ways e\.g\., using shared volumes, S3 etc\. Since the service will require access to S3 and ECR, you must store your AWS credentials as a Kubernetes secret\. This is set as a parameter of the tf\-serving package you installed earlier\. For the purpose of this example, you will use S3 to store and fetch trained models\.

   Check your AWS credentials\. These must have S3 write access\.

   ```
   $ cat ~/.aws/credentials
   ```

1. The output will be something similar to the following:

   ```
   $ [default]
   aws_access_key_id = FAKEAWSACCESSKEYID
   aws_secret_access_key = FAKEAWSSECRETACCESSKEY
   ```

1. Encode the credentials using base64\. Encode the access key first\.

   ```
   $ echo -n 'FAKEAWSACCESSKEYID' | base64
   ```

   Encode the secret access key next\.

   ```
   $ echo -n 'FAKEAWSSECRETACCESSKEYID' | base64
   ```

   Your output should look similar to the following:

   ```
   $ echo -n 'FAKEAWSACCESSKEYID' | base64
   RkFLRUFXU0FDQ0VTU0tFWUlE
   $ echo -n 'FAKEAWSSECRETACCESSKEY' | base64
   RkFLRUFXU1NFQ1JFVEFDQ0VTU0tFWQ==
   ```

1. Create a yaml file to store the secret\. Save it as secret\.yaml in your home directory\.

   ```
   apiVersion: v1
   kind: Secret
   metadata:
     name: aws-s3-secret
   type: Opaque
   data:
     AWS_ACCESS_KEY_ID: RkFLRUFXU0FDQ0VTU0tFWUlE
     AWS_SECRET_ACCESS_KEY: RkFLRUFXU1NFQ1JFVEFDQ0VTU0tFWQ==
   ```

1. Apply the secret to your namespace:

   ```
   $ kubectl -n ${NAMESPACE} apply -f secret.yaml
   ```

1. In this example, you will clone the [tensorflow\-serving](https://github.com/tensorflow/serving/) repository and sync a pretrained model to an S3 bucket\. The following sample names the bucket `tensorflow-serving-models`\. It also syncs a saved model to an S3 bucket called `saved_model_half_plus_two_gpu`\.

   ```
   $ git clone https://github.com/tensorflow/serving/
   $ cd serving/tensorflow_serving/servables/tensorflow/testdata/
   ```

1. Sync the CPU model\.

   ```
   $ aws s3 sync saved_model_half_plus_two_gpu s3://<your_s3_bucket>/saved_model_half_plus_two_gpu
   ```

1. Create the file `tf_inference.yaml`\. Use the contents of the next code block as its content, and update `--model_base_path` to use your S3 bucket\.

   ```
   ---
     kind: Service
     apiVersion: v1
     metadata:
       name: half-plus-two
       labels:
         app: half-plus-two
     spec:
       ports:
       - name: http-tf-serving
         port: 8500
         targetPort: 8500
       - name: grpc-tf-serving
         port: 9000
         targetPort: 9000
       selector:
         app: half-plus-two
         role: master
       type: ClusterIP
     ---
     kind: Deployment
     apiVersion: apps/v1
     metadata:
       name: half-plus-two
       labels:
         app: half-plus-two
         role: master
     spec:
       replicas: 1
       selector:
         matchLabels:
           app: half-plus-two
           role: master
       template:
         metadata:
           labels:
             app: half-plus-two
             role: master
         spec:
           containers:
           - name: half-plus-two
             image: 763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-inference:1.13-gpu-py36-cu100-ubuntu16.04
             command:
             - /usr/bin/tensorflow_model_server
             args:
             - --port=9000
             - --rest_api_port=8500
             - --model_name=saved_model_half_plus_two_gpu
             - --model_base_path=s3://tensorflow-trained-models/saved_model_half_plus_two_gpu
             ports:
             - containerPort: 8500
             - containerPort: 9000
             imagePullPolicy: IfNotPresent
             env:
             - name: AWS_ACCESS_KEY_ID
               valueFrom:
                 secretKeyRef:
                   key: AWS_ACCESS_KEY_ID
                   name: aws-s3-secret
             - name: AWS_SECRET_ACCESS_KEY
               valueFrom:
                 secretKeyRef:
                   key: AWS_SECRET_ACCESS_KEY
                   name: aws-s3-secret
             - name: AWS_REGION
               value: us-east-1
             - name: S3_USE_HTTPS
               value: "true"
             - name: S3_VERIFY_SSL
               value: "true"
             - name: S3_ENDPOINT
               value: s3.us-east-1.amazonaws.com
               resources:
                 limits:
                   cpu: 4
                   memory: 4Gi
                   nvidia.com/gpu: 1
                 requests:
                   cpu: "1"
                   memory: 1Gi
   ```

1. Apply the configuration to a new pod in the previously defined namespace:

   ```
   $ kubectl -n ${NAMESPACE} apply -f tf_inference.yaml
   ```

   Your output should be similar to the following:

   ```
   service/half-plus-two created
   deployment.apps/half-plus-two created
   ```

1. Check status of the pod and wait for the pod to be in “RUNNING” state:

   ```
   $ kubectl get pods -n ${NAMESPACE}
   ```

1. Repeat the check status step until you see the following "RUNNING" state:

   ```
   NAME                     READY     STATUS    RESTARTS   AGE
   half-plus-two-vmwp9  1/1       Running   0          3m
   ```

1. To further describe the pod, you can run:

   ```
   $ kubectl describe pod <pod_name> -n ${NAMESPACE}
   ```

1. Since the serviceType here is ClusterIP, you can forward the port from your container to your host machine \(the ampersand runs this in the background\):

   ```
   $ kubectl port-forward -n ${NAMESPACE} `kubectl get pods -n ${NAMESPACE} --selector=app=half-plus-two -o jsonpath='{.items[0].metadata.name}'` 8500:8500 &
   ```

1. Place the following json string in a file called `half_plus_two_input.json`

   ```
   {"instances": [1.0, 2.0, 5.0]} 
   ```

1. Run inference on the model:

   ```
   $ curl -d @half_plus_two_input.json -X POST http://localhost:8500/v1/models/saved_model_half_plus_two_cpu:predict
   ```

   The expected output is as follows:

   ```
   {
       "predictions": [2.5, 3.0, 4.5
       ]
   }
   ```