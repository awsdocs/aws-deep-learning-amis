# Deep Learning AMI with Source Code \(CUDA 9, Ubuntu\) Version: 2\.0<a name="dlami-source-ubuntu-latest"></a>

## Deep Learning Amazon Machine Image<a name="dlami-source-ubuntu-latest-dplami"></a>

The Deep Learning AMIs are prebuilt with CUDA9 and MXNet and also contain the Anaconda Platform \(Python2 and Python3\)\.

## Highlights of the Release<a name="dlami-source-ubuntu-latest-highlights"></a>

1. Used Deep Learning Base AMI \(Ubuntu\) as the base AMI 

1. Refreshed TensorFlow master with CUDA9/Volta support

1. MXNet upgraded to v1\.0

1. Upgraded PyTorch to v0\.3\.0

1. Added Keras 2\.0\.9 support with TensorFlow as the default backend

## Prebuilt Deep Learning Frameworks<a name="dlami-source-ubuntu-latest-pdlf"></a>
+ [Apache MXNet](http://mxnet.io/): MXNet is a flexible, efficient, portable and scalable open source library for deep learning\. It supports declarative and imperative programming models, across a wide variety of programming languages, making it powerful yet simple to code deep learning applications\. MXNet is efficient, inherently supporting automatic parallel scheduling of portions of source code that can be parallelized over a distributed environment\. MXNet is also portable, using memory optimizations that allow it to run on mobile phones to full servers\.
  + branch/tag used: v1\.0
  + Justification: Stable and well tested
  + Source Directories: 
    + /home/ubuntu/src/mxnet
+ [Caffe2](https://github.com/caffe2/caffe2): Caffe2 is a cross\-platform framework made with expression, speed, and modularity in mind\.
  + branch/tag used: v0\.8\.1 tag
  + Justification: Stable and well tested
  + Note: Available for Python2\.7 only
  + Source Directories: 
    + For Python2\.7\+ \- /home/ec2\-user/src/caffe2
    + For Anaconda Python2\.7\+ \- /home/ec2\-user/src/caffe2\_anaconda2
+ [Keras](https://github.com/fchollet/keras): Keras \- Deep Learning Library for Python\)

  Tensorflow integration with v2\.0\.9\. Tensorflow is the default backend\.
  + Justification : Stable release 
  + Source Directories: 
    + /home/ec2\-user/src/keras
+ [PyTorch](http://pytorch.org/): PyTorch is a python package that provides two high\-level features: Tensor computation \(like numpy\) with strong GPU acceleration, and Deep Neural Networks built on a tape\-based autograd system\.
  + branch/tag used: v0\.3\.0
  + Justification: Stable and well tested
  + Source\_Directories: 
    + /home/ubuntu/src/pytorch
+ [TensorFlow](https://www.tensorflow.org/): TensorFlow™ is an open source software library for numerical computation using data flow graphs\.
  + branch/tag used : Master tag
  + Justification : Stable and well tested
  + Source Directories : 
    + For Python2\.7\+ \- /home/ubuntu/src/caffe
    + For Python3\+ \- /home/ubuntu/src/caffe3
    + For Anaconda Python2\.7\+ \- /home/ubuntu/src/caffe\_anaconda2
    + For Anaconda3 Python3\+ \- /home/ubuntu/src/caffe\_anaconda3
    + For CPU\_ONLY : /home/ubuntu/src/caffe\_cpu

## Python 2\.7 and Python 3\.5 Support<a name="dlami-source-ubuntu-latest-pythonsupport"></a>

Python 2\.7 and Python 3\.5 are supported in the AMI for the following Deep Learning Frameworks:

1. Apache MXNet

1. Caffe2

1. Keras

1. PyTorch

1. Tensorflow

## CPU Instance Type Support<a name="dlami-source-ubuntu-latest-cpu-instance"></a>

The AMI supports CPU Instance Types for all frameworks\. MXNet is built with support for Intel MKL2017 DNN library support\. 

## GPU Drivers Installed<a name="dlami-source-ubuntu-latest-gpu-drivers"></a>
+ CuDNN 7
+ Nvidia 384\.81
+ CUDA 9\.0

## Launching Deep Learning Instance<a name="dlami-source-ubuntu-latest-launching-dl"></a>

Choose the flavor of the AMI from the list below in the region of your choice and follow the steps at:

[EC2 Documentation to launch P2 Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html)

## Testing the FrameWorks<a name="dlami-source-ubuntu-latest-testing-frameworks"></a>

The Deep Learning frameworks have been tested with [MNIST](http://yann.lecun.com/exdb/mnist/) data\. The AMI contains scripts to train and test with MNIST for each of the frameworks\. 

The scripts are available in the /home/ubuntu/src/bin directory\.

The following scripts test the various frameworks:

/home/ubuntu/src/bin/testMXNet : tests MXNet

/home/ubuntu/src/bin/testTensorFlow : tests TensorFlow

/home/ubuntu/src/bin/testCaffe2 : tests Caffe2

The following tests have been run against each of the frameworks:
+ MXNet: This [example](https://github.com/dmlc/mxnet/blob/master/example/image-classification/train_mnist.py) inside the MXNet repository\. Validation accuracy threshold tested for is 97%\.
+ Tensorflow: This [example](https://github.com/fchollet/keras/blob/master/examples/mnist_cnn.py) inside the keras repository\. Validation accuracy threshold tested for is 95%\.
+ Caffe2: Based on this [example](https://github.com/caffe2/tutorials/blob/master/MNIST.ipynb) inside the Caffe2 repository\. Validation accuracy threshold is 90%\.

## AMI Region Availability<a name="dlami-source-ubuntu-latest-regions"></a>

Available in the following regions:


| Region | Code | 
| --- | --- | 
| US East \(Ohio\) | us\-east\-2 | 
| US East \(N\. Virginia\) | us\-east\-1 | 
| GovCloud | us\-gov\-west\-1 | 
| US West \(N\. California\) | us\-west\-1 | 
| US West \(Oregon\) | us\-west\-2 | 
| Beijing \(China\) | cn\-north\-1 | 
| Ningxia \(China\) | cn\-northwest\-1 | 
| Asia Pacific \(Mumbai\) | ap\-south\-1 | 
| Asia Pacific \(Seoul\) | ap\-northeast\-2 | 
| Asia Pacific \(Singapore\) | ap\-southeast\-1 | 
| Asia Pacific \(Sydney\) | ap\-southeast\-2 | 
| Asia Pacific \(Tokyo\) | ap\-northeast\-1 | 
| Canada \(Central\) | ca\-central\-1 | 
| EU \(Frankfurt\) | eu\-central\-1 | 
| EU \(Ireland\) | eu\-west\-1 | 
| EU \(London\) | eu\-west\-2 | 
| EU \(Paris\) | eu\-west\-3 | 
| SA \(Sao Paulo\) | sa\-east\-1 | 

## References<a name="dlami-source-ubuntu-latest-references"></a>
+ [Apache MXNet](https://mxnet.incubator.apache.org/)
+ [Caffe2](https://caffe2.ai)
+ [CNTK](https://www.microsoft.com/en-us/cognitive-toolkit/)
+ [Keras](https://keras.io/)
+ [PyTorch](http://pytorch.org)
+ [TensorFlow](https://www.tensorflow.org)
+ [Theano](http://deeplearning.net/software/theano/)

## Test Environments<a name="dlami-source-ubuntu-latest-test-environments"></a>
+ Built on p2\.16xlarge\.
+ Also tested on p2\.xlarge, c4\.4xlarge\.

## Known Issues<a name="dlami-source-ubuntu-latest-known-issues"></a>
+ **Issue**: NCCL is not fully supported\. Attempts to use NCCL with any instances but P3 will lead to a crash\.

  **Workaround**: Do not use NCCL on instances other than P3\.
+ **Issue**: PyTorch tests are broken\. \~/src/bin/testPyTorch \- installs test environment that is not compatible with the pytorch 0\.3\.0

  **Workaround**: N/A
+ **Issue**: Tutorials provided by the framework or third parties may have Python dependencies not installed on the DLAMI\. 

  **Workaround**: You will need to install those while in the activated environment with conda or pip\.
+ **Issue**: Module not found error running Caffe2 examples\. 

  **Workaround**: [Caffe2 optional dependencies are needed for some tutorials](https://caffe2.ai/docs/getting-started.html)
+ **Issue**: Caffe2 model download features result in 404\. The models have changed locations since the v0\.8\.1 release\. Update models/download\.py to use the [update from master](https://github.com/pytorch/pytorch/blob/master/caffe2/python/models/download.py#L40)\.
+ **Issue**: matplotlib can only render png\. 

  **Workaround**: Install Pillow then restart your kernel\.
+ **Issue**: Changing Caffe2 source code doesn't seem to work\. 

  **Workaround**: Change your PYTHONPATH to use the install location `/usr/local/caffe2` instead of the build folder\.
+ **Issue**: Caffe2 net\_drawer errors\. 

  **Workaround**: Use the [logger patch found in this commit](https://github.com/caffe2/caffe2/commit/2b2744062cf4ac7599c77f70ea61d976629b934a#diff-1de171cab0b3f1f6618b1f26480c5945R29)\.
+ **Issue**: Caffe2 example shows an error regarding LMDB \(can't open DB, etc\.\) 

  **Workaround**: This will require a build from source after installing system LMDB, such as: `sudo apt-get install liblmdb-dev`
+ **Issue**: SSH disconnects while using Jupyter server ties up your local port\. When trying create a tunnel to the server you see `channel_setup_fwd_listener_tcpip: cannot listen to port: 8057`\.

  **Workaround**: Use `lsof -ti:8057 | xargs kill -9` where 8057 is the local port you used\. Then try to create the tunnel to your Jupyter server again\.