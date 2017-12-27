# Features of the DLAMI<a name="features"></a>

**Preinstalled Frameworks**

There are currently three primary flavors of the DLAMI with other variations related to the operating system \(OS\) and software versions: 

+ [Deep Learning AMI with Conda](overview-conda.md) \- frameworks installed separately using `conda` packages and separate Python environments

+ [Deep Learning AMI with Source Code](overview-source.md) \- frameworks installed from source together in the same Python environment

+ [Deep Learning Base AMI](overview-base.md) \- no frameworks installed; only [NVIDIA CUDA](https://developer.nvidia.com/cuda-zone) and other dependencies

The new Deep Learning AMI with Conda uses Anaconda environments to isolate each framework, so you can switch between them at will and not worry about their dependencies conflicting\. The Deep Learning AMI with Source Code has all of the deep learning frameworks installed into the same Python environment, along with the frameworks' source\. Check the release notes on each DLAMI for specifics\. If you're a contributor, you probably want the Deep Learning AMI with Source Code or Deep Learning Base AMI, but if you're a "user" the Conda is more lightweight and easier to use\. 

Regardless of how the frameworks are installed, here are the supported frameworks for all DLAMIs:

+ Apache MXNet with Gluon

+ Caffe\*

+ Caffe2

+ CNTK

+ Keras

+ PyTorch

+ TensorFlow with Keras 2

+ Theano\*\*

+ Torch\*

**Note**  
\* Only available on the Deep Learning AMI with Source Code\.  
\*\* Theano is being phased out, as it is no longer an active project\.

For more information on selecting the best DLAMI for you, take a look at [Getting Started](gs.md)\.

**Preinstalled GPU Software**

Even if you use a CPU\-only instance, the DLAMI will have [NVIDIA CUDA](https://developer.nvidia.com/cuda-zone) and [NVIDIA cuDNN](https://developer.nvidia.com/cudnn)\. The installed software is the same regardless of the instance type\. Keep in mind that GPU\-specific tools only work on an instance that has at least one GPU\. More information on this is covered in the [Selecting the Instance Type for DLAMI](instance-select.md)\.

+ [NVIDIA CUDA](https://developer.nvidia.com/cuda-zone) 9

+ [NVIDIA cuDNN](https://developer.nvidia.com/cudnn) 7