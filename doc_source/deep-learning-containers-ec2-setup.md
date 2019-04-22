# Setup<a name="deep-learning-containers-ec2-setup"></a>

Before you begin, complete the following prerequisites:
+ Create an IAM user or modify an existing user with the following policies\. You can search for them by name in the IAM console's policy tab\. 
  +  [AmazonECS\_FullAccess Policy](https://console.aws.amazon.com/iam/home?region=us-east-1#/policies/arn%3Aaws%3Aiam%3A%3Aaws%3Apolicy%2FAmazonECS_FullAccess) 
  +  [AmazonEC2ContainerRegistryFullAccess](https://console.aws.amazon.com/iam/home?region=us-east-1#/policies/arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryFullAccess) 

  Refer to the [IAM documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_manage-attach-detach.html) for further info on creating or editing an IAM user\.
+ Launch an Amazon EC2 instance \(CPU or GPU\), preferably a Deep Learning AMI with Conda\. Other AMIs will work but require relevant GPU drivers\.
+ Connect to your DLAMI instance via ssh\. Refer to [Troubleshooting Connecting to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/TroubleshootingInstancesConnecting.html) for further information\. 
+ In your Amazon EC2 instance, run aws configure and provide the credentials of your created user\.
+ In your Amazon EC2 instance, run the following command to login to the Amazon ECR repository where AWS Deep Learning Containers images are hosted\.

  ```
  $(aws ecr get-login --no-include-email --region us-east-1 --registry-ids 763104351884)
  ```

  Note: Include the '$' at the beginning of previous command

For a complete list of AWS Deep Learning Containers, refer to [Deep Learning Containers Images](deep-learning-containers-images.md)\. 