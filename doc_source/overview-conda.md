# Deep Learning AMI with Conda<a name="overview-conda"></a>

The Conda DLAMI uses Anaconda virtual environments\. These environments are configured to keep the different framework installations separate\. It also makes it easy to switch between frameworks\. This is great for learning and experimenting with all of the frameworks the DLAMI has to offer\. Most users find that the new Deep Learning AMI with Conda is perfect for them\. 

These "Conda" AMIs will be the primary DLAMIs\. It will be updated often with the latest versions from the frameworks, and have the latest GPU drivers and software\. It will be generally referred to as *the* AWS Deep Learning AMI in most documents\.
+ The Ubuntu 18\.04 DLAMI has the following frameworks: Apache MXNet \(Incubating\), Chainer, PyTorch, TensorFlow, and TensorFlow 2
+ The Ubuntu 16\.04 and Amazon Linux DLAMI has the following frameworks: Apache MXNet \(Incubating\), Chainer, Keras, PyTorch, TensorFlow, and TensorFlow 2
+ The Amazon Linux 2 DLAMI has the following frameworks: Apache MXNet \(Incubating\), Chainer, PyTorch, TensorFlow, TensorFlow 2, and Keras

**Note**  
We no longer include the CNTK, Caffe, Caffe2 and Theano Conda environments in the AWS Deep Learning AMI starting with the v28 release\. Previous releases of the AWS Deep Learning AMI that contain these environments will continue to be available\. However, we will only provide updates to these environments if there are security fixes published by the open source community for these frameworks\.

## Stable versus Release Candidates<a name="overview-conda-stability"></a>

The Conda AMIs use optimized binaries of the most recent formal releases from each framework\. Release candidates and experimental features are not to be expected\. The optimizations depend on the framework's support for acceleration technologies like Intel's MKL DNN, which will speed up training and inference on C5 and C4 CPU instance types\. The binaries are also compiled to support advanced Intel instruction sets including, but not limited to AVX, AVX\-2, SSE4\.1, and SSE4\.2\. These accelerate vector and floating point operations on Intel CPU architectures\. Additionally, for GPU instance types, the CUDA and cuDNN will be updated with whichever version the latest official release supports\. 

The Deep Learning AMI with Conda automatically installs the most optimized version of the framework for your EC2 instance upon the framework's first activation\. For more information, refer to [Using the Deep Learning AMI with Conda](tutorial-conda.md)\. 

If you want to install from source, using custom or optimized build options, the [Deep Learning Base AMI](overview-base.md)'s might be a better option for you\.

## Python 2 Deprecation<a name="overview-conda-python2"></a>

The Python open source community has officially ended support for Python 2 on January 1, 2020\. The TensorFlow and PyTorch community have announced that the TensorFlow 2\.1 and PyTorch 1\.4 releases will be the last ones supporting Python 2\. Previous releases of the DLAMI \(v26, v25, etc\) that contain Python 2 Conda environments will continue to be available\. However, we will provide updates to the Python 2 Conda environments on previously published DLAMI versions only if there are security fixes published by the open source community for those versions\. DLAMI releases with the next versions of the TensorFlow and PyTorch frameworks will not contain the Python 2 Conda environments\.

## Elastic Inference Support<a name="overview-conda-ei"></a>

The Deep Learning AMI with Conda comes with environments that support Elastic Inference for both [AWS Deep Learning AMI, Ubuntu 16\.04 Options](ubuntu.md) and [AWS Deep Learning AMI Amazon Linux Options](al.md)\. Elastic Inference environments are not currently supported for [AWS Deep Learning AMI, Ubuntu 18\.04 Options](ubuntu18-04.md) and [AWS Deep Learning AMI Amazon Linux 2 Options](al2.md)\. For tutorials and more information on Elastic Inference, see the [Elastic Inference Documentation](https://docs.aws.amazon.com/elastic-inference/latest/developerguide/what-is-ei.html)\.

## CUDA Support<a name="overview-conda-cuda"></a>

The Deep Learning AMI with Conda's CUDA version and the frameworks supported for each:
+ CUDA 10\.1 with cuDNN 7: Apache MXNet
+ CUDA 10 with cuDNN 7: PyTorch, TensorFlow, TensorFlow 2, Apache MXNet, Chainer

Specific framework version numbers can be found in the [Release Notes for DLAMI](appendix-ami-release-notes.md)

Choose this DLAMI type or learn more about the different DLAMIs with the Next Up option\.
+ [Deep Learning AMI with Conda Options](conda.md)

**Next Up**  
[Deep Learning Base AMI](overview-base.md)

## Related Topics<a name="w95aab7b5c13c19"></a>
+ For a tutorial on using a Deep Learning AMI with Conda refer to the [Using the Deep Learning AMI with Conda](tutorial-conda.md) tutorial\.