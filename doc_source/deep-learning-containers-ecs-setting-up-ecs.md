# Setting Up Amazon ECS for AWS Deep Learning Containers<a name="deep-learning-containers-ecs-setting-up-ecs"></a>

This section explains how to set up Amazon ECS to use AWS Deep Learning Containers\. Run the following actions from your host\.

1. Create an Amazon ECS cluster in the Region that contains the key pair and security group that you created previously\.

   ```
   aws ecs create-cluster --cluster-name ecs-ec2-training-inference --region us-east-1
   ```

1. Launch one or more Amazon EC2 instances into your cluster\. For GPU\-based work, refer to [Working with GPUs on Amazon ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-gpu.html) to guide your instance type selection\. After selecting your instance type, select an ECS\-optimized AMI that fits your use case\. For CPU\-based work, you can use the Amazon Linux or Amazon Linux 2 ECS\-optimized AMIs\. For GPU\-based work, you must use the ECS GPU\-optimized AMI and a p2/p3 instance type\. Amazon ECS\-optimized AMI IDs can be found here for [Amazon ECS\-optimized AMIs](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-optimized_AMI.html)\. In this example, we're launching one instance with a GPU\-based AMI and 100 GB of disk size in us\-east\-1\. 

   1. Create a file named `my_script.txt` with the following contents\. Reference the same cluster name that you created in the previous step\.

      ```
      #!/bin/bash
      echo ECS_CLUSTER=ecs-ec2-training-inference >> /etc/ecs/ecs.config
      ```

   1. \(Optional\) Create a file named `my_mapping.txt` with the following content, which changes the size of the root volume after the instance is created\.

      ```
      [
          {
              "DeviceName": "/dev/xvda",
              "Ebs": {
                  "VolumeSize": 100
              }
          }
      ]
      ```

   1. Launch an Amazon EC2 instance with the Amazon ECS\-optimized AMI and attach it to the cluster\. Use your security group ID and key pair name that you created and replace them in the following command\. To get the latest Amazon ECS\-optimized AMI ID, see [Amazon ECS\-optimized AMIs](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs-optimized_AMI.html) in the *Amazon Elastic Container Service Developer Guide*\.

      ```
      aws ec2 run-instances --image-id ami-0dfdeb4b6d47a87a2 \
                             --count 1 \
                             --instance-type p2.8xlarge \
                             --key-name key-pair-1234 \
                             --security-group-ids sg-abcd1234 \
                             --iam-instance-profile Name="ecsInstanceRole" \
                             --user-data file://my_script.txt \
                             --block-device-mapping file://my_mapping.txt \
                             --region us-east-1
      ```

      In the Amazon EC2 console, you can verify that this step was successful by the `instance-id` from the response\.

You now have an Amazon ECS cluster with container instances running\. Verify that the Amazon EC2 instances are registered with the cluster with the following steps\.

**To verify that the Amazon EC2 instance is registered with the cluster**

1. Open the Amazon ECS console at [https://console\.aws\.amazon\.com/ecs/](https://console.aws.amazon.com/ecs/)\.

1. Select the cluster that you registered your Amazon EC2 instances with\.

1. On the **Cluster** page, choose **ECS Instances**\.

1. Verify that the **Agent Connected** value is **True** for the `instance-id` created in previous step\. Also, note the CPU available and Memory available from the console as these values can be useful in the following tutorials\. It might take a few minutes to appear in the console\.