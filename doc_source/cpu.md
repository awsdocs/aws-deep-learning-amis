# Recommended CPU Instances<a name="cpu"></a>

Whether you're on a budget, learning about deep learning, or just want to run a prediction service, you have many affordable options in the CPU category\. Some frameworks take advantage of Intel's MKL DNN, which will speed up training and inference on C5 \(not available in all regions\), C4, and C3 CPU instance types\. It will also speed up inference on GPU instance types\.
+ [Amazon EC2 C5 Instances](https://aws.amazon.com/ec2/instance-types/c5/) have up to 72 Intel vCPUs\.
+ Amazon EC2 C4 Instances have up to 36 Intel vCPUs\.
+ Check out [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/) and look for **Compute Optimized** to see the different CPU instance options\.

**Note**  
C5 instances \(not available in all regions\) excel at scientific modelling, batch processing, distributed analytics, high\-performance computing \(HPC\), and machine/deep learning inference\.

**Note**  
The size of your model should be a factor in selecting an instance\. If your model exceeds an instance's available RAM, select a different instance type with enough memory for your application\. 

**Next Up**  
[Pricing for the DLAMI](pricing.md)