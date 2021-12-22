# Recommended GPU Instances<a name="gpu"></a>

We recommend a GPU instance for most deep learning purposes\. Training new models is faster on a GPU instance than a CPU instance\. You can scale sub\-linearly when you have multi\-GPU instances or if you use distributed training across many instances with GPUs\. To set up distributed training, see [Distributed Training](distributed-training.md)\. 

The following instance types support the DLAMI\. For information about GPU instance type options and their uses, see [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/) and select **Accelerated Computing**\.

**Note**  
The size of your model should be a factor in selecting an instance\. If your model exceeds an instance's available RAM, select a different instance type with enough memory for your application\. 
+ [Amazon EC2 P3 Instances](https://aws.amazon.com/ec2/instance-types/p3/) have up to 8 NVIDIA Tesla V100 GPUs\.
+ [Amazon EC2 P4 Instances](https://aws.amazon.com/ec2/instance-types/p4/) have up to 8 NVIDIA Tesla A100 GPUs\.
+ [Amazon EC2 G3 Instances](https://aws.amazon.com/ec2/instance-types/g3/) have up to 4 NVIDIA Tesla M60 GPUs\.
+ [Amazon EC2 G4 Instances](https://aws.amazon.com/ec2/instance-types/g4/) have up to 4 NVIDIA T4 GPUs\.
+ [Amazon EC2 G5 Instances](https://aws.amazon.com/ec2/instance-types/g5/) have up to 8 NVIDIA A10G GPUs\.
+ [Amazon EC2 G5g Instances](http://aws.amazon.com/ec2/instance-types/g5g/) have Arm\-based [AWS Graviton2 processors](http://aws.amazon.com/ec2/graviton/)\.

DLAMI instances provide tooling to monitor and optimize your GPU processes\. For more information about monitoring your GPU processes, see [GPU Monitoring and Optimization](tutorial-gpu.md)\.

For specific tutorials on working with G5g instances, see [The Graviton DLAMI](tutorial-graviton.md)\.

**Next Up**  
[Recommended CPU Instances](cpu.md)