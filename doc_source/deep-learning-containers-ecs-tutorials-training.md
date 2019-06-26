# Training<a name="deep-learning-containers-ecs-tutorials-training"></a>

This section will guide you on how to run training on AWS Deep Learning ContainersAWS Deep Learning Containers for ECS using MXNet and TensorFlow\.

For a complete list of AWS Deep Learning Containers, refer to [Deep Learning Containers Images](deep-learning-containers-images.md)\. 

**Note**  
MKL users: read the [AWS Deep Learning Containers MKL Recommendations](deep-learning-containers-mkl.md) to get the best training or inference performance\.

**Topics**
+ [TensorFlow Training](#deep-learning-containers-ecs-tutorials-training-tf)
+ [MXNet Training](#deep-learning-containers-ecs-tutorials-training-mxnet)

## TensorFlow Training<a name="deep-learning-containers-ecs-tutorials-training-tf"></a>

Before you can run a task on your ECS cluster, you must register a task definition\. Task definitions are lists of containers grouped together\. The following example uses a sample Docker image that adds training scripts to AWS Deep Learning Containers\.

1. Create a file named `ecs-deep-learning-container-training-taskdef.json` with the following contents\.
   + For CPU:

     ```
     {
     	"requiresCompatibilities": [
     		"EC2"
     	],
     	"containerDefinitions": [{
     		"command": [
     			"mkdir -p /test && cd /test && git clone https://github.com/fchollet/keras.git && chmod +x -R /test/ && python keras/examples/mnist_cnn.py"
     		],
     		"entryPoint": [
     			"sh",
     			"-c"
     		],
     		"name": "tensorflow-training-container",
     		"image": "763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-training:1.13-cpu-py36-ubuntu16.04",
     		"memory": 4000,
     		"cpu": 256,
     		"essential": true,
     		"portMappings": [{
     			"containerPort": 80,
     			"protocol": "tcp"
     		}],
     		"logConfiguration": {
     			"logDriver": "awslogs",
     			"options": {
     				"awslogs-group": "awslogs-tf-ecs",
     				"awslogs-region": "us-east-1",
     				"awslogs-stream-prefix": "tf",
     				"awslogs-create-group": "true"
     			}
     		}
     	}],
     	"volumes": [],
     	"networkMode": "bridge",
     	"placementConstraints": [],
     	"family": "TensorFlow"
     }
     ```
   + For GPU

     ```
     {
           "requiresCompatibilities": [
             "EC2"
           ],
           "containerDefinitions": [
             {
         "command": [
                     "mkdir -p /test && cd /test && git clone https://github.com/fchollet/keras.git && chmod +x -R /test/ && python keras/examples/mnist_cnn.py"
                  ],
                  "entryPoint": [
                     "sh",
                     "-c"
                  ],
               "name": "tensorflow-training-container",
               "image": "763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-training:1.13-horovod-gpu-py36-cu100-ubuntu16.04",
               "memory": 6111,
               "cpu": 256,
               "resourceRequirements" : [{
                 "type" : "GPU",
                 "value" : "1"
               }],
               "essential": true,
               "portMappings": [
                 {
                   "containerPort": 80,
                   "protocol": "tcp"
                 }
               ],
               "logConfiguration": {
                   "logDriver": "awslogs",
                   "options": {
                       "awslogs-group": "awslogs-tf-ecs",
                       "awslogs-region": "us-east-1",
                       "awslogs-stream-prefix": "tf",
           				"awslogs-create-group": "true"
                   }
               }
             }
           ],
           "volumes": [],
           "networkMode": "bridge",
           "placementConstraints": [],
           "family": "tensorflow-training"
         }
     ```

1. Register the task definition\. Note the revision number in the output\.

   ```
   aws ecs register-task-definition --cli-input-json file://ecs-deep-learning-container-training-taskdef.json
   ```

1. Create a task using the task definition\. You need the revision ID from the previous step\.

   ```
   aws ecs run-task --cluster ecs-ec2-training-inference --task-definition tf:1
   ```

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Select the `ecs-ec2-training-inference` cluster\.

1. On the **Cluster** page, choose **Tasks**\.

1. Once your task is in a `RUNNING` state, choose the task ID\.

1. Under **Containers**, expand the container details\.

1. Under **Log Configuration**, choose **View logs in CloudWatch**\. This takes you to the CloudWatch console to view the training progress logs\.

## MXNet Training<a name="deep-learning-containers-ecs-tutorials-training-mxnet"></a>

Before you can run a task on your ECS cluster, you must register a task definition\. Task definitions are lists of containers grouped together\. The following example uses a sample Docker image that adds training scripts to AWS Deep Learning Containers\.

1. Create a file named `ecs-deep-learning-container-training-taskdef.json` with the following contents\.
   + For CPU:

     ```
     {
        "requiresCompatibilities":[
           "EC2"
        ],
        "containerDefinitions":[
           {
              "command":[
                 "git clone -b 1.4 https://github.com/apache/incubator-mxnet.git && python /incubator-mxnet/example/image-classification/train_mnist.py"
              ],
              "entryPoint":[
                 "sh",
                 "-c"
              ],
              "name":"mxnet-training",
              "image":"763104351884.dkr.ecr.us-east-1.amazonaws.com/mxnet-training:1.4.1-cpu-py36-ubuntu16.04",
              "memory":4000,
              "cpu":256,
              "essential":true,
              "portMappings":[
                 {
                    "containerPort":80,
                    "protocol":"tcp"
                 }
              ],
              "logConfiguration":{
                 "logDriver":"awslogs",
                 "options":{
                    "awslogs-group":"/ecs/mxnet-training-cpu",
                    "awslogs-region":"us-east-1",
                    "awslogs-stream-prefix":"mnist",
                    "awslogs-create-group":"true"
                 }
              }
           }
        ],
        "volumes":[
     
        ],
        "networkMode":"bridge",
        "placementConstraints":[
     
        ],
        "family":"mxnet"
     }
     ```
   +  For GPU:

     ```
     {
        "requiresCompatibilities":[
           "EC2"
        ],
        "containerDefinitions":[
           {
              "command":[
                 "git clone -b 1.4 https://github.com/apache/incubator-mxnet.git && python /incubator-mxnet/example/image-classification/train_mnist.py --gpus 0"
              ],
              "entryPoint":[
                 "sh",
                 "-c"
              ],
              "name":"mxnet-training",
              "image":"763104351884.dkr.ecr.us-east-1.amazonaws.com/mxnet-training:1.4.1-gpu-py36-cu100-ubuntu16.04",
              "memory":4000,
              "cpu":256,
              "resourceRequirements":[
                 {
                    "type":"GPU",
                    "value":"1"
                 }
              ],
              "essential":true,
              "portMappings":[
                 {
                    "containerPort":80,
                    "protocol":"tcp"
                 }
              ],
              "logConfiguration":{
                 "logDriver":"awslogs",
                 "options":{
                    "awslogs-group":"/ecs/mxnet-training-gpu",
                    "awslogs-region":"us-east-1",
                    "awslogs-stream-prefix":"mnist",
                    "awslogs-create-group":"true"
                 }
              }
           }
        ],
        "volumes":[
     
        ],
        "networkMode":"bridge",
        "placementConstraints":[
     
        ],
        "family":"mxnet-training"
     }
     ```

1. Register the task definition\. Note the revision number in the output\.

   ```
   aws ecs register-task-definition --cli-input-json file://ecs-deep-learning-container-training-taskdef.json
   ```

1. Create a task using the task definition\. You need the revision ID from the previous step\.

   ```
   aws ecs run-task --cluster ecs-ec2-training-inference --task-definition mx:1
   ```

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Select the `ecs-ec2-training-inference` cluster\.

1. On the **Cluster** page, choose **Tasks**\.

1. Once your task is in a `RUNNING` state, choose the task ID\.

1. Under **Containers**, expand the container details\.

1. Under **Log Configuration**, choose **View logs in CloudWatch**\. This takes you to the CloudWatch console to view the training progress logs\.