# Release Note Details for Deep Learning AMI \(Ubuntu\) Version 1\.0<a name="CONDA_UBUNTU1"></a>

## AWS Deep Learning AMI<a name="CONDA_UBUNTU1-dplami"></a>

The AWS Deep Learning AMI are prebuilt with CUDA 8 and 9, and several deep learning frameworks\. The DLAMI uses the Anaconda Platform with both Python2 and Python3 to easily switch between frameworks\.

## Highlights of the Release<a name="CONDA_UBUNTU1-highlights"></a>

1. Used Ubuntu 2017\.09 \(ami\-8c1be5f6\) as the base AMI 

1. CUDA 9

1. CuDNN 7

1. NCCL 2\.0\.5

1. CuBLAS 8 and 9

1. glibc 2\.18

1. OpenCV 3\.2\.0

## Pre\-built Deep Learning Frameworks<a name="CONDA_UBUNTU1-pdlf"></a>
+ [Apache MXNet](http://mxnet.io/): MXNet is a flexible, efficient, portable and scalable open source library for deep learning\. It supports declarative and imperative programming models, across a wide variety of programming languages, making it powerful yet simple to code deep learning applications\. MXNet is efficient, inherently supporting automatic parallel scheduling of portions of source code that can be parallelized over a distributed environment\. MXNet is also portable, using memory optimizations that allow it to run on mobile phones to full servers\.
  + branch/tag used: v0\.12\.0
  + Justification: Stable and well tested
  + To activate: 
    + For Anaconda Python2\.7\+ \- `source activate mxnet_p27`
    + For Anaconda Python3\+ \- `source activate mxnet_p36`
+ [Caffe2](https://github.com/caffe2/caffe2): Caffe2 is a cross\-platform framework made with expression, speed, and modularity in mind\.
  + branch/tag used: v0\.8\.1
  + Justification: Stable and well tested
  + Note: Available for Python2\.7 only
  + To activate: 
    + For Anaconda Python2\.7\+ \- `source activate caffe2_p27`
+ [CNTK](https://github.com/Microsoft/CNTK/): CNTK \- Microsoft Cognitive Toolkit \- is a unified deep\-learning toolkit by Microsoft Research\.
  + branch/tag used : v2\.2
  + Justification : Latest release
  + To activate: 
    + For Anaconda Python2\.7\+ \- `source activate cntk_p27`
    + For Anaconda Python3\+ \- `source activate cntk_p36`
+ [Keras](https://github.com/fchollet/keras): Keras \- Deep Learning Library for Python\)

  Tensorflow integration with v2\.0\.9
  + Justification : Stable release 
  + To activate: 
    + For Anaconda Python2\.7\+ \- `source activate tensorflow_p27`
    + For Anaconda Python3\+ \- `source activate tensorflow_p36`

  MXNet integration with v1\.2\.2
  + Justification : Stable release 
  + To activate: 
    + For Anaconda Python2\.7\+ \- `source activate mxnet_p27`
    + For Anaconda Python3\+ \- `source activate mxnet_p36`
+ [PyTorch](https://pytorch.org/): PyTorch is a python package that provides two high\-level features: Tensor computation \(like numpy\) with strong GPU acceleration, and Deep Neural Networks built on a tape\-based autograd system 
  + branch/tag used : v0\.2
  + Justification : Stable and well tested
  + To activate: 
    + For Anaconda Python2\.7\+ \- `source activate pytorch_p27`
    + For Anaconda Python3\+ \- `source activate pytorch_p36`
+ [TensorFlow](https://www.tensorflow.org/): TensorFlow™ is an open source software library for numerical computation using data flow graphs\.
  + branch/tag used : v1\.4
  + Justification : Stable and well tested
  + To activate: 
    + For Anaconda Python2\.7\+ \- `source activate tensorflow_p27`
    + For Anaconda Python3\+ \- `source activate tensorflow_p36`
+ [Theano](http://deeplearning.net/software/theano/): Theano is a Python library that allows you to define, optimize, and evaluate mathematical expressions involving multi\-dimensional arrays efficiently\.
  + branch/tag used: v0\.9
  + Justification: Stable and well tested
  + To activate: 
    + For Anaconda Python2\.7\+ \- `source activate theano_p27`
    + For Anaconda Python3\+ \- `source activate theano_p36`

## Python 2\.7 and Python 3\.5 Support<a name="CONDA_UBUNTU1-pythonsupport"></a>

Python 2\.7 and Python 3\.6 are supported in the AMI for all of this installed Deep Learning Frameworks except Caffe2:

1. Apache MXNet

1. Caffe2 \(Python 2\.7 only\)

1. CNTK

1. PyTorch

1. Tensorflow

1. Theano

## CPU Instance Type Support<a name="CONDA_UBUNTU1-cpu-instance"></a>

The AMI supports CPU Instance Types for all frameworks\. MXNet is built with support for Intel MKL2017 DNN library support\. 

## GPU Drivers Installed<a name="CONDA_UBUNTU1-gpu-drivers"></a>
+ CuDNN 7
+ NVIDIA 384\.81
+ CUDA 9\.0

## Launching Deep Learning Instance<a name="CONDA_UBUNTU1-launching-dl"></a>

Choose the flavor of the AMI from the list below in the region of your choice and follow the steps at:

[AWS Deep Learning AMI Developer Guide](http://docs.aws.amazon.com/dlami/latest/devguide/gs.html)

## Testing the Frameworks<a name="CONDA_UBUNTU1-testing-frameworks"></a>

The deep learning frameworks have been tested with [MNIST](http://yann.lecun.com/exdb/mnist/) data\. The AMI contains scripts to train and test with MNIST for each of the frameworks\. 

The scripts are available in the /home/ubuntu/src/bin directory\.

## Deep Learning AMI \(Ubuntu\) Regions<a name="CONDA_UBUNTU1-regions"></a>

Deep Learning AMI \(Ubuntu\)s are available in the following regions:
+ US East \(Ohio\): us\-east\-2
+ US East \(N\. Virginia\): us\-east\-1
+ US West \(N\. California\): us\-west\-1
+ US West \(Oregon\): us\-west\-2
+ Asia Pacific \(Seoul\): ap\-northeast\-2
+ Asia Pacific \(Singapore\): ap\-southeast\-1
+ Asia Pacific \(Tokyo\): ap\-northeast\-1
+ EU \(Ireland\): eu\-west\-1

## References<a name="CONDA_UBUNTU1-references"></a>
+ [Apache MXNet](https://mxnet.incubator.apache.org/)
+ [Caffe2](https://caffe2.ai)
+ [CNTK](https://www.microsoft.com/en-us/cognitive-toolkit/)
+ [Keras](https://keras.io/)
+ [PyTorch](http://pytorch.org)
+ [TensorFlow](https://www.tensorflow.org)
+ [Theano](http://deeplearning.net/software/theano/)

## Test Environments<a name="CONDA_UBUNTU1-test-environments"></a>
+ Built on p2\.16xlarge\.
+ Also tested on p2\.xlarge, c4\.4xlarge\.

## Deep Learning AMI \(Ubuntu\) Known Issues<a name="CONDA_UBUNTU1-known-issues"></a>
+ **Issue**: Tutorials provided by the framework or third parties may have Python dependencies not installed on the DLAMI\. 

  **Workaround**: You will need to install those while in the activated environment with conda or pip\.
+ **Issue**: Module not found error running Caffe2 examples\. 

  **Workaround**: [Caffe2 optional dependencies are needed for some tutorials](https://caffe2.ai/docs/getting-started.html)
+ **Issue**: Caffe2 model download features result in 404\. The models have changed locations since the v0\.8\.1 release\. Update models/download\.py to use the [update from master](https://github.com/caffe2/caffe2/blob/master/caffe2/python/models/download.py#L40)\.
+ **Issue**: matplotlib can only render png\. 

  **Workaround**: Install Pillow then restart your kernel\.
+ **Issue**: Changing Caffe2 source code doesn't seem to work\. 

  **Workaround**: Change your PYTHONPATH to use the install location `/usr/local/caffe2` instead of the build folder\.
+ **Issue**: Caffe2 net\_drawer errors\. 

  **Workaround**: Use the [logger patch found in this commit](https://github.com/caffe2/caffe2/commit/2b2744062cf4ac7599c77f70ea61d976629b934a#diff-1de171cab0b3f1f6618b1f26480c5945R29)\.
+ **Issue**: Caffe2 example shows an error regarding LMDB \(can't open DB, etc\.\) 

  **Workaround**: This will require a build from source after installing system LMDB, such as: `sudo apt-get install liblmdb-dev`
+ **Issue**: SSH disconnects while using Jupyter server ties up your local port\. When trying create a tunnel to the server you see `channel_setup_fwd_listener_tcpip: cannot listen to port: 8157`\.

  **Workaround**: Use `lsof -ti:8057 | xargs kill -9` where 8057 is the local port you used\. Then try to create the tunnel to your Jupyter server again\.

  **Problem**: The MOTD has an incorrect link to these release notes\.

  **Workaround**: If you made here, then you already know\.

  A future release will address this issue\. 