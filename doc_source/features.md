# Features of the DLAMI<a name="features"></a>

## Preinstalled Frameworks<a name="features-frameworks"></a>

There are currently three primary flavors of the DLAMI with other variations related to the operating system \(OS\) and software versions: 
+ [Deep Learning AMI with Conda](overview-conda.md) \- frameworks installed separately using `conda` packages and separate Python environments
+ [Deep Learning Base AMI](overview-base.md) \- no frameworks installed; only [NVIDIA CUDA](https://developer.nvidia.com/cuda-zone) and other dependencies

The new Deep Learning AMI with Conda uses Anaconda environments to isolate each framework, so you can switch between them at will and not worry about their dependencies conflicting\.

For more information on selecting the best DLAMI for you, take a look at [Getting Started](gs.md)\.

This is the full list of supported frameworks by Deep Learning AMI with Conda:
+ Apache MXNet
+ Caffe
+ Caffe2\*\*
+ CNTK\*\*
+ Keras
+ PyTorch
+ TensorFlow
+ Theano\*\*

## Preinstalled GPU Software<a name="features-gpu"></a>

Even if you use a CPU\-only instance, the DLAMI will have [NVIDIA CUDA](https://developer.nvidia.com/cuda-zone) and [NVIDIA cuDNN](https://developer.nvidia.com/cudnn)\. The installed software is the same regardless of the instance type\. Keep in mind that GPU\-specific tools only work on an instance that has at least one GPU\. More information on this is covered in the [Selecting the Instance Type for DLAMI](instance-select.md)\.
+ [NVIDIA CUDA](https://developer.nvidia.com/cuda-zone) 9
+ [NVIDIA cuDNN](https://developer.nvidia.com/cudnn) 7
+ CUDA 8 is available as well\. See the [CUDA Installations and Framework Bindings](overview-cuda.md) for more information\.

## Model Serving and Visualization<a name="features-gpu"></a>

Deep Learning AMI with Conda comes preinstalled with two kinds of model servers, one for MXNet and one for TensorFlow, as well as TensorBoard, for model visualizations\.
+ [Model Server for Apache MXNet \(MMS\)](tutorial-mms.md)
+ [TensorFlow Serving](tutorial-tfserving.md)
+ [TensorBoard](tutorial-tensorboard.md)