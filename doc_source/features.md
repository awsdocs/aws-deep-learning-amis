# Features of the DLAMI<a name="features"></a>

## Preinstalled Frameworks<a name="features-frameworks"></a>

There are currently two primary flavors of the DLAMI with other variations related to the operating system \(OS\) and software versions: 
+ [Deep Learning AMI with Conda](overview-conda.md) \- frameworks installed separately using `conda` packages and separate Python environments
+ [Deep Learning Base AMI](overview-base.md) \- no frameworks installed; only [NVIDIA CUDA](https://developer.nvidia.com/cuda-zone) and other dependencies

The Deep Learning AMI with Conda uses Anaconda environments to isolate each framework, so you can switch between them at will and not worry about their dependencies conflicting\.

For more information on selecting the best DLAMI for you, take a look at [Getting Started](gs.md)\.

This is the full list of supported frameworks by Deep Learning AMI with Conda:
+ Apache MXNet
+ Caffe
+ Caffe2
+ Chainer
+ CNTK
+ Keras
+ PyTorch
+ TensorFlow
+ TensorFlow 2
+ Theano

**Note**  
We no longer include the CNTK, Caffe, Caffe2 and Theano Conda environments in the AWS Deep Learning AMI starting with the v28 release\. Previous releases of the AWS Deep Learning AMI that contain these environments will continue to be available\. However, we will only provide updates to these environments if there are security fixes published by the open source community for these frameworks\.

## Preinstalled GPU Software<a name="features-gpu"></a>

Even if you use a CPU\-only instance, the DLAMI will have [NVIDIA CUDA](https://developer.nvidia.com/cuda-zone) and [NVIDIA cuDNN](https://developer.nvidia.com/cudnn)\. The installed software is the same regardless of the instance type\. Keep in mind that GPU\-specific tools only work on an instance that has at least one GPU\. More information on this is covered in the [Selecting the Instance Type for DLAMI](instance-select.md)\.
+ Latest version of [NVIDIA CUDA](https://developer.nvidia.com/cuda-zone)
+ Latest version of [NVIDIA cuDNN](https://developer.nvidia.com/cudnn)
+ Older versions of CUDA are available as well\. See the [CUDA Installations and Framework Bindings](overview-cuda.md) for more information\.

## Elastic Inference Support<a name="features-ei"></a>

The Deep Learning AMI with Conda comes with environments that support Elastic Inference for both [AWS Deep Learning AMI, Ubuntu 16\.04 Options](ubuntu.md) and [AWS Deep Learning AMI Amazon Linux Options](al.md)\. Elastic Inference environments are not currently supported for [AWS Deep Learning AMI, Ubuntu 18\.04 Options](ubuntu18-04.md) and [AWS Deep Learning AMI Amazon Linux 2 Options](al2.md)\. For tutorials and more information on Elastic Inference, see the [Elastic Inference Documentation](https://docs.aws.amazon.com/elastic-inference/latest/developerguide/what-is-ei.html)\.

## Model Serving and Visualization<a name="features-gpu"></a>

Deep Learning AMI with Conda comes preinstalled with two kinds of model servers, one for MXNet and one for TensorFlow, as well as TensorBoard, for model visualizations\.
+ [Model Server for Apache MXNet \(MMS\)](tutorial-mms.md)
+ [TensorFlow Serving](tutorial-tfserving.md)
+ [TensorBoard](tutorial-tensorboard.md)