# The Graviton DLAMI<a name="tutorial-graviton"></a>

AWS Graviton GPU DLAMIs are designed to provide high performance and cost efficiency for deep learning workloads\. Specifically, the G5g instance type features the Arm\-based [AWS Graviton2 processor](http://aws.amazon.com/ec2/graviton/), which was built from the ground up by AWS and optimized for how customers run their workloads in the cloud\. AWS Graviton GPU DLAMIs are pre\-configured with Docker, NVIDIA Docker, NVIDIA Driver, CUDA, CuDNN, NCCL, and TensorRT, as well as popular machine learning frameworks such as TensorFlow and PyTorch\.

With the G5g instance type, you can take advantage of the price and performance benefits of Graviton2 to deploy GPU\-accelerated deep learning models at a significantly lower cost when compared with x86\-based instances with GPU acceleration\.

## Select a Graviton DLAMI<a name="tutorial-graviton-select-dlami"></a>

Launch a [G5g instance](http://aws.amazon.com/ec2/instance-types/g5g/) with the Graviton DLAMI of your choice\. 

For step\-by\-step instructions on launching a DLAMI, see [Launching and Configuring a DLAMI\.](https://docs.aws.amazon.com/dlami/latest/devguide/launch-config.html) 

For a list of the most recent Graviton DLAMIs, see the [Release Notes for DLAMI](https://docs.aws.amazon.com/dlami/latest/devguide/appendix-ami-release-notes.html)\.

## Get Started<a name="tutorial-graviton-get-started"></a>

The following topics show you how to get started using the Graviton DLAMI\. 

**Topics**
+ [Select a Graviton DLAMI](#tutorial-graviton-select-dlami)
+ [Get Started](#tutorial-graviton-get-started)
+ [Using the Graviton GPU DLAMI](tutorial-graviton-cuda.md)
+ [Using the Graviton GPU TensorFlow DLAMI](tutorial-graviton-tensorflow.md)
+ [Using the Graviton GPU PyTorch DLAMI](tutorial-graviton-pytorch.md)