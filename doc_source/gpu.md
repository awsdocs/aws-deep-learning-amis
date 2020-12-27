# Recommended GPU Instances<a name="gpu"></a>

A GPU instance is recommended for most deep learning purposes\. Training new models will be faster on a GPU instance than a CPU instance\. You can scale sub\-linearly when you have multi\-GPU instances or if you use distributed training across many instances with GPUs\. To set up distributed training, see [Distributed Training](distributed-training.md)\. 

**Note**  
The size of your model should be a factor in selecting an instance\. If your model exceeds an instance's available RAM, select a different instance type with enough memory for your application\. 
+ [Amazon EC2 P3 Instances](https://aws.amazon.com/ec2/instance-types/p3/) have up to 8 NVIDIA Tesla V100 GPUs\.
+ [Amazon EC2 P2 Instances](https://aws.amazon.com/ec2/instance-types/p2/) have up to 16 NVIDIA K80 GPUs\.
+ [Amazon EC2 G3 Instances](https://aws.amazon.com/ec2/instance-types/g3/) have up to 4 NVIDIA Tesla M60 GPUs\.
+ [Amazon EC2 G4 Instances](https://aws.amazon.com/ec2/instance-types/g4/) have up to 4 NVIDIA T4 GPUs\.
+ Check out [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/) and choose Accelerated Computing to see the different GPU instance options\.

DLAMI instances provide tooling to monitor and optimize your GPU processes\. For more information on overseeing your GPU processes, see [GPU Monitoring and Optimization](tutorial-gpu.md)\.

**Next Up**  
[Recommended CPU Instances](cpu.md)
