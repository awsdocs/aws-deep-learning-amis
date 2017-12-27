# Deep Learning AMI Ubuntu Version: 1\.0<a name="Ubuntu1_0"></a>

## Deep Learning Amazon Machine Image<a name="ubuntu-dplami-1"></a>

The Deep Learning AMIs are pre\-built with popular Deep Learning frameworks and also contain the Anaconda Platform \(Python2 and Python3\)\.

## Pre\-built Deep Learning Frameworks<a name="ubuntu-pdlf-1"></a>

+ [MXNet](http://mxnet.io/): MXNet is a flexible, efficient, portable and scalable open source library for deep learning\. It supports declarative and imperative programming models, across a wide variety of programming languages, making it powerful yet simple to code deep learning applications\. MXNet is efficient, inherently supporting automatic parallel scheduling of portions of source code that can be parallelized over a distributed environment\. MXNet is also portable, using memory optimizations that allow it to run on mobile phones to full servers\.

  + branch/tag used: v0\.9\.3 tag

  + Justification: Stable and well tested

  + Source\_Directories: 

    + /home/ubuntu/src/mxnet

+ [Caffe](http://caffe.berkeleyvision.org/): Caffe is a deep learning framework made with expression, speed, and modularity in mind\. It is developed by the Berkeley Vision and Learning Center \(BVLC\) and by community contributors\.

  + branch/tag used: master branch

  + Justification: Supports cuda7\.5 and cudnn 5\.1

  + Source\_Directories: 

    + For Python2\.7\+ \- /home/ubuntu/src/caffe

    + For Python3\+ \- /home/ubuntu/src/caffe3

    + For Anaconda Python2\.7\+ \- /home/ubuntu/src/caffe\_anaconda2

    + For Anaconda3 Python3\+ \- /home/ubuntu/src/caffe\_anaconda3

    + For CPU\_ONLY : /home/ubuntu/src/caffe\_cpu

+ [Theano](http://deeplearning.net/software/theano/): Theano is a Python library that allows you to define, optimize, and evaluate mathematical expressions involving multi\-dimensional arrays efficiently\.

  + branch/tag used: rel\-0\.8\.2 tag

  + Justification: Stable and well tested

  + Source\_Directories: 

    + /home/ubuntu/src/theano

+ [TensorFlow](https://www.tensorflow.org/): TensorFlow™ is an open source software library for numerical computation using data flow graphs\.

  + branch/tag used : v0\.12\.1 tag

  + Justification : Stable and well tested

  + Source\_Directories : 

    + For Python2\.7\+ \- /home/ubuntu/src/tensorflow

    + For Python3\+ \- /home/ubuntu/src/tensorflow3

    + For Anaconda Python2\.7\+ \- /home/ubuntu/src/tensorflow\_anaconda

    + For Anaconda Python3\+ \- /home/ubuntu/src/tensorflow\_anaconda3

+ [Torch](http://torch.ch/): Torch is a scientific computing framework with wide support for machine learning algorithms that puts GPUs first\. It is easy to use and efficient, thanks to an easy and fast scripting language, LuaJIT, and an underlying C/CUDA implementation\.

  + branch/tag used : master branch

  + Justification : No other stable branch or tag available

  + Source\_Directories : 

    + /home/ubuntu/src/torch

+ [CNTK](https://github.com/Microsoft/CNTK/): CNTK \- Microsoft Cognitive Toolkit \- is a unified deep\-learning toolkit by Microsoft Research\.

  + branch/tag used : v2\.0beta7\.0 tag

  + Justification : Latest release

  + Source\_Directories : 

    + /home/ubuntu/src/cntk

## Python 2\.7 and Python 3\.5 Support<a name="ubuntu-pythonsupport-1"></a>

Python 2\.7 and Python 3\.5 are supported in the AMI for the following Deep Learning Frameworks:

1. Caffe

1. Tensorflow

1. Theano

1. MXNet

1. CNTK

## CPU Instance Type Support<a name="ubuntu-cpu-instance-1"></a>

The AMI supports CPU Instance Types for all frameworks\. MXNet is built with support for Intel MKL2017 DNN library support\. If you want to use the caffe binary for the CPU instance, then you should use the binary inside /home/ubuntu/src/caffe\_cpu/

## CNTK Python Support<a name="ubuntu-cntk-pythonsupport-1"></a>

You can run CNTK for Python inside a conda environment\. To do this:

```
cd /home/ubuntu/src/anaconda3/bin
    source activate cntk-py34
```

## GPU Drivers Installed<a name="ubuntu-gpu-drivers-1"></a>

+ CuDNN 5\.1

+ NVIDIA 352\.99

+ CUDA 7\.5

## Launching Deep Learning Instance<a name="ubuntu-launching-dl-1"></a>

Choose the flavor of the AMI from the list below in the region of your choice and follow the steps at:

[EC2 Documentation to launch G2 Instance](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html)

## Testing the Frameworks<a name="ubuntu-testing-frameworks-1"></a>

The Deep Learning frameworks have been tested with [MNIST](http://yann.lecun.com/exdb/mnist/) data\. The AMI contains scripts to train and test with MNIST for each of the frameworks\. The test checks if the validation accuracy is above a specific threshold\. The threshold is different for each of the frameworks\.

The scripts are available in the /home/ec2\-user/src/bin directory\.

The following scripts test the various frameworks:

/home/ubuntu/src/bin/testAll : tests all frameworks

/home/ubuntu/src/bin/testMXNet : tests MXNet

/home/ubuntu/src/bin/testTheano : tests Theano

/home/ubuntu/src/bin/testTensorFlow : tests TensorFlow

/home/ubuntu/src/bin/testTorch : tests Torch

/home/ubuntu/src/bin/testCNTK : tests CNTK

The following tests have been run against each of the frameworks:

+ MXNet: This [example](https://github.com/dmlc/mxnet/blob/master/example/image-classification/train_mnist.py) inside the MXNet repository\. Validation accuracy threshold tested for is 97%\.

+ Tensorflow: This [example](https://github.com/fchollet/keras/blob/master/examples/mnist_cnn.py) inside the keras repository\. Validation accuracy threshold tested for is 95%\.

+ Theano: The same [example](https://github.com/fchollet/keras/blob/master/examples/mnist_cnn.py) above\. Validation accuracy threshold is 95%\.

+ Torch: This [example](https://github.com/torch/demos/blob/master/train-a-digit-classifier/train-on-mnist.lua) inside the Torch tree\. Validation accuracy threshold is 93%\.

+ Caffe: This [example](https://github.com/BVLC/caffe/blob/master/examples/mnist/train_lenet.sh) inside the Caffe repository\. Validation accuracy threshold is 98%\.

+ CNTK: This [example](https://github.com/Microsoft/CNTK/blob/master/Examples/Image/GettingStarted/03_OneConvDropout.cntk) inside the CNTK repository\. Validation accuracy threshold is 97%\.

## Ubuntu AMI<a name="ubuntu-ami-1"></a>

Ubuntu based Deep Learning AMIs are available in the following regions:

+ eu\-west\-1\(DUB\)

+ us\-east\-1\(IAD\)

+ us\-west\-1\(PDX\)

## References<a name="ubuntu-references-1"></a>

[MXNet](http://mxnet.io/)

[Caffe](http://caffe.berkeleyvision.org/)

[Theano](http://deeplearning.net/software/theano/)

[TensorFlow](https://www.tensorflow.org/)

[Torch](http://torch.ch/)

[CNTK](https://github.com/Microsoft/CNTK)

## Test Environments<a name="ubuntu-test-environments-1"></a>

+ Built and Tested on g2\.2xlarge\.

+ Also tested on g2\.8xlarge, p2\.16xlarge, c4\.4xlarge\.