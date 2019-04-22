# Inference<a name="deep-learning-containers-ecs-tutorials-inference"></a>

This section will guide you on how to run inference on AWS Deep Learning Containers for ECS using MXNet and TensorFlow\.

For a complete list of AWS Deep Learning Containers, refer to [Deep Learning Containers Images](deep-learning-containers-images.md)\. 

**Topics**
+ [TensorFlow Inference](#deep-learning-containers-ecs-tutorials-inference-tf)
+ [MXNet Inference](#deep-learning-containers-ecs-tutorials-inference-mxnet)

## TensorFlow Inference<a name="deep-learning-containers-ecs-tutorials-inference-tf"></a>

The following examples use a sample Docker image that adds either CPU or GPU inference scripts to AWS Deep Learning Containers\.

### CPU\-based Inferece<a name="deep-learning-containers-ecs-tutorials-inference-tf-cpu"></a>

Use the following example to run CPU\-based inference\.

1. Create a file named `ecs-dlc-cpu-inference-taskdef.json` with the following contents\.

   ```
   {
   	"requiresCompatibilities": [
   		"EC2"
   	],
   	"containerDefinitions": [{
   		"command": [
   			"mkdir -p /test && cd /test && git clone -b r1.13 https://github.com/tensorflow/serving.git && tensorflow_model_server --port=8500 --rest_api_port=8501 --model_name=saved_model_half_plus_two_cpu --model_base_path=/test/serving/tensorflow_serving/servables/tensorflow/testdata/saved_model_half_plus_two_cpu"
   		],
   		"entryPoint": [
   			"sh",
   			"-c"
   		],
   		"name": "EC2TFInference",
   		"image": "763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-inference:1.13-cpu-py36-ubuntu16.04",
   		"memory": 8111,
   		"cpu": 256,
   		"essential": true,
   		"portMappings": [{
   				"hostPort": 8500,
   				"protocol": "tcp",
   				"containerPort": 8500
   			},
   			{
   				"hostPort": 8501,
   				"protocol": "tcp",
   				"containerPort": 8501
   			},
   			{
   				"containerPort": 80,
   				"protocol": "tcp"
   			}
   		],
   		"logConfiguration": {
   			"logDriver": "awslogs",
   			"options": {
   				"awslogs-group": "/ecs/TFInference",
   				"awslogs-region": "us-west-2",
   				"awslogs-stream-prefix": "ecs",
   				"awslogs-create-group": "true"
   			}
   		}
   	}],
   	"volumes": [],
   	"networkMode": "bridge",
   	"placementConstraints": [],
   	"family": "Ec2TFInference"
   }
   ```

1. Register the task definition\. Note the revision number in the output\.

   ```
   aws ecs register-task-definition --cli-input-json file://ecs-dlc-cpu-inference-taskdef.json
   ```

1. Create an Amazon ECS service\. When specifying the task definition, replace `revision_id` with the revision number of the task definition from the output of the previous step\.

   ```
   aws ecs create-service --cluster ecs-ec2-training-inference \
                          --service-name cli-ec2-inference-cpu \
                          --task-definition Ec2TFInference:revision_id \
                          --desired-count 1 \
                          --launch-type EC2 \
                          --scheduling-strategy="REPLICA" \
                          --region us-east-1
   ```

1. Verify the service and get the network endpoint by completing the following steps\.

   1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

   1. Select the `ecs-ec2-training-inference` cluster\.

   1. On the **Cluster** page, choose **Services** and then **cli\-ec2\-inference\-cpu**\.

   1. Once your task is in an `RUNNING` state, choose the task ID\.

   1. Under **Containers**, expand the container details\.

   1. Under **Name** and then **Network Bindings**, under **External Link** note the IP address for port 8501\.

   1. Under **Log Configuration**, choose **View logs in CloudWatch**\. This takes you to the CloudWatch console to view the training progress logs\.

1. To run inference, use the following command, replacing the external IP address with the external link IP address from the previous step\.

   ```
   curl -d '{"instances": [1.0, 2.0, 5.0]}' -X POST http://<External ip>:8501/v1/models/saved_model_half_plus_two_cpu:predict
   ```

   The following is sample output\.

   ```
   {
       "predictions": [2.5, 3.0, 4.5
       ]
   }
   ```
**Important**  
If you are unable to connect to the external IP address, be sure that your corporate firewall is not blocking non\-standards ports, like 8501\. You can try switching to a guest network to verify\.

### GPU\-Based Inference<a name="deep-learning-containers-ecs-tutorials-inference-tf-gpu"></a>

 Use the following example to run GPU\-based inference\.

1. Create a file named `ecs-dlc-gpu-inference-taskdef.json` with the following contents\.

   ```
   {
   	"requiresCompatibilities": [
   		"EC2"
   	],
   	"containerDefinitions": [{
   		"command": [
   			"mkdir -p /test && cd /test && git clone -b r1.13 https://github.com/tensorflow/serving.git && tensorflow_model_server --port=8500 --rest_api_port=8501 --model_name=saved_model_half_plus_two_gpu --model_base_path=/test/serving/tensorflow_serving/servables/tensorflow/testdata/saved_model_half_plus_two_gpu"
   		],
   		"entryPoint": [
   			"sh",
   			"-c"
   		],
   		"name": "EC2TFInference",
   		"image": "763104351884.dkr.ecr.us-east-1.amazonaws.com/tensorflow-inference:1.13-gpu-py36-cu100-ubuntu16.04",
   		"memory": 8111,
   		"cpu": 256,
   		"resourceRequirements": [{
   			"type": "GPU",
   			"value": "1"
   		}],
   		"essential": true,
   		"portMappings": [{
   				"hostPort": 8500,
   				"protocol": "tcp",
   				"containerPort": 8500
   			},
   			{
   				"hostPort": 8501,
   				"protocol": "tcp",
   				"containerPort": 8501
   			},
   			{
   				"containerPort": 80,
   				"protocol": "tcp"
   			}
   		],
   		"logConfiguration": {
   			"logDriver": "awslogs",
   			"options": {
   				"awslogs-group": "/ecs/TFInference",
   				"awslogs-region": "us-east-1",
   				"awslogs-stream-prefix": "ecs",
   				"awslogs-create-group": "true"
   			}
   		}
   	}],
   	"volumes": [],
   	"networkMode": "bridge",
   	"placementConstraints": [],
   	"family": "Ec2TFInference"
   }
   ```

1. Register the task definition\. Note the revision number in the output\.

   ```
   aws ecs register-task-definition --cli-input-json file://ecs-dlc-gpu-inference-taskdef.json
   ```

1. Create an Amazon ECS service\. When specifying the task definition, replace `revision_id` with the revision number of the task definition from the output of the previous step\.

   ```
   aws ecs create-service --cluster ecs-ec2-training-inference \
                          --service-name cli-ec2-inference-gpu \
                          --task-definition Ec2TFInference:revision_id \
                          --desired-count 1 \
                          --launch-type EC2 \
                          --scheduling-strategy="REPLICA" \
                          --region us-east-1
   ```

1. Verify the service and get the network endpoint by completing the following steps\.

   1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

   1. Select the `ecs-ec2-training-inference` cluster\.

   1. On the **Cluster** page, choose **Services** and then **cli\-ec2\-inference\-cpu**\.

   1. Once your task is in an `RUNNING` state, choose the task ID\.

   1. Under **Containers**, expand the container details\.

   1. Under **Name** and then**Network Bindings**, under **External Link** note the IP address for port 8501\.

   1. Under **Log Configuration**, choose **View logs in CloudWatch**\. This takes you to the CloudWatch console to view the training progress logs\.

1. To run inference, use the following command, replacing the external IP address with the external link IP address from the previous step\.

   ```
   curl -d '{"instances": [1.0, 2.0, 5.0]}' -X POST http://<External ip>:8501/v1/models/saved_model_half_plus_two_gpu:predict
   ```

   The following is sample output\.

   ```
   {
       "predictions": [2.5, 3.0, 4.5
       ]
   }
   ```
**Important**  
If you are unable to connect to the external IP address, be sure that your corporate firewall is not blocking non\-standards ports, like 8501\. You can try switching to a guest network to verify\.

## MXNet Inference<a name="deep-learning-containers-ecs-tutorials-inference-mxnet"></a>

Before you can run a task on your ECS cluster, you must register a task definition\. Task definitions are lists of containers grouped together\. The following examples use a sample Docker image that adds either CPU or GPU inference scripts to AWS Deep Learning Containers\.

### CPU\-Based Inference<a name="deep-learning-containers-ecs-tutorials-inference-mxnet-cpu"></a>

 Use the following task definition to run CPU\-based inference\.

1. Create a file named `ecs-dlc-cpu-inference-taskdef.json` with the following contents\.

   ```
   {
   	"requiresCompatibilities": [
   		"EC2"
   	],
   	"containerDefinitions": [{
   		"command": [
   			"mxnet-model-server --start --mms-config /home/model-server/config.properties --models  squeezenet=https://s3.amazonaws.com/model-server/models/squeezenet_v1.1/squeezenet_v1.1.model"
   		],
   		"name": "ECSMXInference",
   		"image": "763104351884.dkr.ecr.us-east-1.amazonaws.com/mxnet-inference:1.4.0-cpu-py36-ubuntu16.04",
   		"memory": 8111,
   		"cpu": 256,
   		"essential": true,
   		"portMappings": [{
   				"hostPort": 8081,
   				"protocol": "tcp",
   				"containerPort": 8081
   			},
   			{
   				"hostPort": 80,
   				"protocol": "tcp",
   				"containerPort": 8080
   			}
   		],
   		"logConfiguration": {
   			"logDriver": "awslogs",
   			"options": {
   				"awslogs-group": "/ecs/mxnet-inference-cpu",
   				"awslogs-region": "us-east-1",
   				"awslogs-stream-prefix": "ecs",
   				"awslogs-create-group": "true"
   			}
   		}
   	}],
   	"volumes": [],
   	"networkMode": "bridge",
   	"placementConstraints": [],
   	"family": "EcsMXInference"
   }
   ```

1. Register the task definition\. Note the revision number in the output\.

   ```
   aws ecs register-task-definition --cli-input-json file://ecs-dlc-cpu-inference-taskdef.json
   ```

1. Create an Amazon ECS service\. When specifying the task definition, replace `revision_id` with the revision number of the task definition from the output of the previous step\.

   ```
   aws ecs create-service --cluster ecs-ec2-training-inference \
                          --service-name cli-ec2-inference-cpu \
                          --task-definition Ec2TFInference:revision_id \
                          --desired-count 1 \
                          --launch-type EC2 \
                          --scheduling-strategy REPLICA \
                          --region us-east-1
   ```

1. Verify the service and get the endpoint\.

   1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

   1. Select the `ecs-ec2-training-inference` cluster\.

   1. On the **Cluster** page, choose **Services** and then **cli\-ec2\-inference\-cpu**\.

   1. Once your task is in an `RUNNING` state, choose the task ID\.

   1. Under **Containers**, expand the container details\.

   1. Under **Name** and then **Network Bindings**, under **External Link** note the IP address for port 8501\.

   1. Under **Log Configuration**, choose **View logs in CloudWatch**\. This takes you to the CloudWatch console to view the training progress logs\.

1. To run inference, use the following command\.

   ```
   curl -O https://s3.amazonaws.com/model-server/inputs/kitten.jpg
   curl -X POST http://<External ip>/predictions/squeezenet -T kitten.jpg
   ```

   The following is sample output\.

   ```
   [
     {
       "probability": 0.8582226634025574,
       "class": "n02124075 Egyptian cat"
     },
     {
       "probability": 0.09160050004720688,
       "class": "n02123045 tabby, tabby cat"
     },
     {
       "probability": 0.037487514317035675,
       "class": "n02123159 tiger cat"
     },
     {
       "probability": 0.0061649843119084835,
       "class": "n02128385 leopard, Panthera pardus"
     },
     {
       "probability": 0.003171598305925727,
       "class": "n02127052 lynx, catamount"
     }
   ]
   ```
**Important**  
If you are unable to connect to the external IP address, be sure that your corporate firewall is not blocking non\-standards ports, like 8501\. You can try switching to a guest network to verify\.

### GPU\-Based inference<a name="deep-learning-containers-ecs-tutorials-inference-mxnet-gpu"></a>

 Use the following task definition to run GPU\-based inference\.

```
{
	"requiresCompatibilities": [
		"EC2"
	],
	"containerDefinitions": [{
		"command": [
			"mxnet-model-server --start --mms-config /home/model-server/config.properties --models  squeezenet=https://s3.amazonaws.com/model-server/models/squeezenet_v1.1/squeezenet_v1.1.model"
		],
		"name": "ECSMXInference",
		"image": "763104351884.dkr.ecr.us-east-1.amazonaws.com/mxnet-inference:1.4.0-gpu-py36-cu90-ubuntu16.04",
		"memory": 8111,
		"cpu": 256,
		"resourceRequirements": [{
			"type": "GPU",
			"value": "1"
		}],
		"essential": true,
		"portMappings": [{
				"hostPort": 8081,
				"protocol": "tcp",
				"containerPort": 8081
			},
			{
				"hostPort": 80,
				"protocol": "tcp",
				"containerPort": 8080
			}
		],
		"logConfiguration": {
			"logDriver": "awslogs",
			"options": {
				"awslogs-group": "/ecs/mxnet-inference-gpu",
				"awslogs-region": "us-east-1",
				"awslogs-stream-prefix": "ecs",
				"awslogs-create-group": "true"
			}
		}
	}],
	"volumes": [],
	"networkMode": "bridge",
	"placementConstraints": [],
	"family": "EcsMXInference"
}
```

1. Use the following command to register the task definition\. Note the output of the revision number\.

   ```
   aws ecs register-task-definition --cli-input-json file://<Task definition file>
   ```

1. To create the service, replace the `revision_id` with the output from the previous step in the following command\.

   ```
   aws ecs create-service --cluster ecs-ec2-training-inference \
                       --service-name cli-ec2-inference-gpu \
                       --task-definition Ec2TFInference:<revision_id> \
                       --desired-count 1 \
                       --launch-type "EC2" \
                       --scheduling-strategy REPLICA \
                       --region us-east-1
   ```

1. Verify the service and get the endpoint\.

   1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

   1. Select the `ecs-ec2-training-inference` cluster\.

   1. On the **Cluster** page, choose **Services** and then **cli\-ec2\-inference\-cpu**\.

   1. Once your task is in an `RUNNING` state, choose the task ID\.

   1. Under **Containers**, expand the container details\.

   1. Under **Name** and then **Network Bindings**, under **External Link** note the IP address for port 8501\.

   1. Under **Log Configuration**, choose **View logs in CloudWatch**\. This takes you to the CloudWatch console to view the training progress logs\.

1. To run inference, use the following command\.

   ```
   curl -O https://s3.amazonaws.com/model-server/inputs/kitten.jpg
   curl -X POST http://<External ip>/predictions/squeezenet -T kitten.jpg
   ```

   The following is sample output\.

   ```
   [
     {
       "probability": 0.8582226634025574,
       "class": "n02124075 Egyptian cat"
     },
     {
       "probability": 0.09160050004720688,
       "class": "n02123045 tabby, tabby cat"
     },
     {
       "probability": 0.037487514317035675,
       "class": "n02123159 tiger cat"
     },
     {
       "probability": 0.0061649843119084835,
       "class": "n02128385 leopard, Panthera pardus"
     },
     {
       "probability": 0.003171598305925727,
       "class": "n02127052 lynx, catamount"
     }
   ]
   ```
**Important**  
If you are unable to connect to the external IP address, be sure that your corporate firewall is not blocking non\-standards ports, like 8501\. You can try switching to a guest network to verify\.