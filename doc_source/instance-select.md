# Selecting the Instance Type for DLAMI<a name="instance-select"></a>

 Selecting the instance type can be another challenge, but we'll make this easier for you with a few pointers on how to choose the best one\. Remember, the DLAMI is free, but the underlying compute resources are not\. 
+  If you're new to deep learning, then you probably want an "entry\-level" instance with a single GPU\.
+  If you're budget conscious, then you will need to start a bit smaller and look at the CPU\-only instances\. 
+  If you're interested in running a trained model for inference and predictions \(and not training\), then you might want to run [Amazon Elastic Inference](https://docs.aws.amazon.com/elastic-inference/latest/developerguide/what-is-ei.html)\. This will give you access to a fraction of a GPU, so you can scale affordably\. For high\-volume inference services, you may find that a CPU instance with a lot of memory, or even a cluster of these, is a better solution for you\. 
+  If you're interested in training a model with a lot of data, then you might want a larger instance or even a cluster of GPU instances\. 
+  If you’re interested in running Machine Learning applications using NVIDIA Collective Communications Library \(NCCL\) requiring high levels of inter\-node communications at scale, you might want to use [ Elastic Fabric Adapter \(EFA\)](https://docs.aws.amazon.com/dlami/latest/devguide/tutorial-efa-launching.html)\.

 DLAMIs are not available in every region, but it is possible to copy DLAMIs to the region of your choice\. See [Copying an AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/CopyingAMIs.html) for more info\. Each region supports a different range of instance types and often an instance type has a slightly different cost in different regions\. On each DLAMI main page, you will see a list of instance costs\. Note the region selection list and be sure you pick a region that's close to you or your customers\. If you plan to use more than one DLAMI and potentially create a cluster, be sure to use the same region for all of nodes in the cluster\. 
+ For more detail on instances, check out [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)\.
+ For a more info on regions, visit [EC2 Regions](https://docs.aws.amazon.com/general/latest/gr/rande.html#ec2_region)\.

So with all of those points in mind, make note of the instance type that best applies to your use case and budget\. The rest of the topics in this guide help further inform you and go into more detail\. 

**Important**  
The Deep Learning AMIs include drivers, software, or toolkits developed, owned, or provided by NVIDIA Corporation\. You agree to use these NVIDIA drivers, software, or toolkits only on Amazon EC2 instances that include NVIDIA hardware\.

**Topics**
+ [Recommended GPU Instances](gpu.md)
+ [Recommended CPU Instances](cpu.md)
+ [Pricing for the DLAMI](pricing.md)

**Next Up**  
[Recommended GPU Instances](gpu.md)