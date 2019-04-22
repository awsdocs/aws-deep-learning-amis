# Release Note Details for Deep Learning Base AMI \(Ubuntu\) Version 1\.0<a name="BASE_UBUNTU1"></a>

## AWS Deep Learning AMI<a name="dplami-a"></a>

The Deep Learning Base AMI are prebuilt with CUDA 8 and 9 and ready for your custom deep learning setup\. The Deep Learning Base AMI uses the Anaconda Platform with both Python2 and Python3\.

## Highlights of the Release<a name="highlights-a"></a>

1. Used Ubuntu 2017\.09 \(ami\-8c1be5f6\) as the base AMI 

1. CUDA 9

1. CuDNN 7

1. NCCL 2\.0\.5

1. CuBLAS 8 and 9

1. glibc 2\.18

1. OpenCV 3\.2\.0

## Python 2\.7 and Python 3\.5 Support<a name="pythonsupport-a"></a>

Python 2\.7 and Python 3\.6 are supported in the AMI\.

## CPU Instance Type Support<a name="cpu-instance-a"></a>

The AMI supports CPU Instance Types\.

## GPU Drivers Installed<a name="gpu-drivers-a"></a>
+ Nvidia 384\.81
+ CUDA 9
+ CuDNN 7

## Launching Deep Learning Instance<a name="launching-dl-a"></a>

Choose the flavor of the AMI from the list below in the region of your choice and follow the steps at:

[AWS Deep Learning AMI Developer Guide](https://docs.aws.amazon.com/dlami/latest/devguide/gs.html)

## Deep Learning AMI \(Ubuntu\)<a name="amazonlinuxami-a"></a>

This AMI is available in the following regions:
+ US East \(Ohio\): us\-east\-2
+ US East \(N\. Virginia\): us\-east\-1
+ US West \(N\. California\): us\-west\-1
+ US West \(Oregon\): us\-west\-2
+ Asia Pacific \(Seoul\): ap\-northeast\-2
+ Asia Pacific \(Singapore\): ap\-southeast\-1
+ Asia Pacific \(Tokyo\): ap\-northeast\-1
+ EU \(Ireland\): eu\-west\-1

## Test Environments<a name="test-environments-a"></a>
+ Built on p2\.16xlarge\.
+ Also tested on p2\.xlarge, c4\.4xlarge\.

## Deep Learning Base AMI \(Ubuntu\) Known Issues<a name="BASE_UBUNTU-known-issues"></a>
+ **Issue**: The MOTD has an incorrect link to these release notes\.

  **Workaround**: If you made here, then you already know\.

  A future release will address this issue\. 