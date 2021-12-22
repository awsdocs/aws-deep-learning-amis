# Recommended Habana Instances<a name="habana"></a>

Instances with Habana accelerators are designed to provide high performance and cost efficiency for deep learning model training workloads\. Specifically, DL1 instance types use Habana Gaudi accelerators from Habana Labs, an Intel company\. DL1 instances are ideal for training machine learning models used in applications such as natural language processing, object detection and classification, recommendation engines, and autonomous vehicle perception\.

Instances with Habana accelerators are configured with Habana SynapseAI software and pre\-integrated with popular machine learning frameworks such as TensorFlow and PyTorch\. If you are looking for an optimal combination of performance and price for training deep learning models, consider instances with Habana accelerators for the lowest cost to train\.

**Note**  
The size of your model should be a factor in selecting an instance\. If your model exceeds an instance's available RAM, select a different instance type with enough memory for your application\. 
+ [Amazon EC2 DL1 Instances](https://aws.amazon.com/ec2/instance-types/dl1/) have up to eight Habana Gaudi accelerators, 256GB of accelerator memory, 4TB of local NVMe storage, and 400 Gbps of networking throughput\.

For more information about getting started with Habana DLAMIs, see [The Habana DLAMI](tutorial-habana.md)\.