# Prerequisites<a name="deep-learning-containers-ecs-setup-prerequisites"></a>

This tutorial assumes that you have completed the following prerequisites:
+ The latest version of the AWS CLI is installed and configured\. For more information about installing or upgrading the AWS CLI, see [Installing the AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)\.
+ You have completed the steps in [Setting Up with Amazon ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/get-set-up-for-amazon-ecs.html)\.
+ One of the following must be true:
  + Your user has administrator access\. For more information, see [Setting Up with Amazon ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/get-set-up-for-amazon-ecs.html)\.
  + Your user has the IAM permissions to create a service role\. For more information, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html)\.
  + A user with administrator access has manually created these IAM roles so that they're available on the account to be used\. For more information, see [Amazon ECS Service Scheduler IAM Role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service_IAM_role.html) and [Amazon ECS Container Instance IAM Role](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/instance_IAM_role.html) in the *Amazon Elastic Container Service Developer Guide*\.
+ The Amazon CloudWatch Logs IAM policy is added to the Amazon ECS Container Instance IAM role, which allows Amazon ECS to send logs to Amazon CloudWatch\. For more information, see [CloudWatch Logs IAM Policy](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_cloudwatch_logs.html) in the *Amazon Elastic Container Service Developer Guide*\.
+ You have generated a key pair\. For more information see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)\. 
+ You have created a new security group or updated an existing security group to have the ports open necessary for your desired inference server\.
  + MXNet inference: ports 80 and 8081 open to TCP traffic
  + TensorFlow inference: ports 8501 and 8500 open to TCP traffic

  For more information see [Amazon EC2 Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html)\.