# Deep Learning CUDA 9 AMI Ubuntu Version: 1\.0<a name="CUDA9_Ubuntu1"></a>

## Deep Learning Amazon Machine Image<a name="ubuntu-dplami-a"></a>

The Deep Learning AMIs are prebuilt with CUDA9 and MXNet and also contain the Anaconda Platform \(Python2 and Python3\)\.

## Highlights of the Release<a name="ubuntu-highlights-a"></a>

1. Used Ubuntu 16\.04 \(ami\-d15a75c7\) as the base AMI

1. CUDA 9

1. CuDNN 7

1. NCCL 2\.0

1. MXNet with CUDA9 Support

## Prebuilt Deep Learning Frameworks<a name="ubuntu-pdlf-a"></a>
+ [MXNet](http://mxnet.io/): MXNet is a flexible, efficient, portable and scalable open source library for deep learning\. It supports declarative and imperative programming models, across a wide variety of programming languages, making it powerful yet simple to code deep learning applications\. MXNet is efficient, inherently supporting automatic parallel scheduling of portions of source code that can be parallelized over a distributed environment\. MXNet is also portable, using memory optimizations that allow it to run on mobile phones to full servers\.
  + branch/tag used: v0\.12\.0 Release Candidate tag
  + Justification: Stable and well tested
  + Source\_Directories: 
    + /home/ubuntu/src/mxnet
+ [Caffe2](https://github.com/caffe2/caffe2): Caffe2 is a cross\-platform framework made with expression, speed, and modularity in mind\.
  + branch/tag used: v0\.8\.1 tag
  + Justification: Stable and well tested
  + Note: Available for Python2\.7 only
  + Source\_Directories: 
    + For Python2\.7\+ \- /home/ec2\-user/src/caffe2
    + For Anaconda Python2\.7\+ \- /home/ec2\-user/src/caffe2\_anaconda2
+ [TensorFlow](https://www.tensorflow.org/): TensorFlow™ is an open source software library for numerical computation using data flow graphs\.
  + branch/tag used : Master tag
  + Justification : Stable and well tested
  + Source\_Directories : 
    + For Python2\.7\+ \- /home/ubuntu/src/caffe
    + For Python3\+ \- /home/ubuntu/src/caffe3
    + For Anaconda Python2\.7\+ \- /home/ubuntu/src/caffe\_anaconda2
    + For Anaconda3 Python3\+ \- /home/ubuntu/src/caffe\_anaconda3
    + For CPU\_ONLY : /home/ubuntu/src/caffe\_cpu

## Python 2\.7 and Python 3\.5 Support<a name="ubuntu-pythonsupport-a"></a>

Python 2\.7 and Python 3\.5 are supported in the AMI for the following Deep Learning Frameworks:

1. MXNet

1. Caffe2

1. Tensorflow

## CPU Instance Type Support<a name="ubuntu-cpu-instance-a"></a>

The AMI supports CPU Instance Types for all frameworks\. MXNet is built with support for Intel MKL2017 DNN library support\. 

## GPU Drivers Installed<a name="ubuntu-gpu-drivers-a"></a>
+ CuDNN 7
+ Nvidia 384\.81
+ CUDA 9\.0

## Launching Deep Learning Instance<a name="ubuntu-launching-dl-a"></a>

Choose the flavor of the AMI from the list below in the region of your choice and follow the steps at:

[EC2 Documentation to launch P2 Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html)

## Testing the FrameWorks<a name="ubuntu-testing-frameworks-a"></a>

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

## Ubuntu AMI<a name="ubuntu-ami-a"></a>

Ubuntu based Deep Learning AMIs are available in the following regions:
+ eu\-west\-1\(DUB\)
+ us\-east\-1\(IAD\)
+ us\-west\-1\(PDX\)
+ us\-east\-2\(CHM\)
+ ap\-southeast\-2\(SYD\)
+ ap\-northeast\-1\(NRT\)
+ ap\-northeast\-2\(ICN\)

## References<a name="ubuntu-references-a"></a>

[MXNet](http://mxnet.io/)

## Test Environments<a name="ubuntu-test-environments-a"></a>
+ Built on p2\.16xlarge\.
+ Also tested on p2\.xlarge, c4\.4xlarge\.

## Known Issues<a name="known-issues-a"></a>
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
+ **Issue**: SSH disconnects while using Jupyter server ties up your local port\. When trying create a tunnel to the server you see `channel_setup_fwd_listener_tcpip: cannot listen to port: 8157`\.

  **Workaround**: Use `lsof -ti:8057 | xargs kill -9` where 8057 is the local port you used\. Then try to create the tunnel to your Jupyter server again\.