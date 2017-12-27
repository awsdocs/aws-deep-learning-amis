# Recommended GPU Instances<a name="gpu"></a>

A GPU instance is recommended for most deep learning purposes\. Training new models will be faster on a GPU instance than a CPU instance\. You can scale sub\-linearly when you have multi\-GPU instances or if you use distributed training across many instances with GPUs\. 

+ [Amazon EC2 P3 Instances](https://aws.amazon.com/ec2/instance-types/p3/) have up to 8 NVIDIA Tesla V100 GPUs\.

+ [Amazon EC2 P2 Instances](https://aws.amazon.com/ec2/instance-types/p2/) have up to 16 NVIDIA NVIDIA K80 GPUs\.

+ [Amazon EC2 G3 Instances](https://aws.amazon.com/ec2/instance-types/g3/) have up to 4 NVIDIA Tesla M60 GPUs\.

+ Check out [EC2 Instance Types](https://aws.amazon.com/instance-types/) and choose Accelerated Computing to see the different GPU instance options\.

**Note**  
P3 instance type is not yet supported for Windows AMIs\.

**Next Up**  
[Recommended CPU Instances](cpu.md)