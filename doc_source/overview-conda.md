# Deep Learning AMI with Conda<a name="overview-conda"></a>

The newest DLAMI uses Anaconda virtual environments\. These environments are configured to keep the different framework installations separate\. It also makes it easy to switch between frameworks\. This is great for learning and experimenting with all of the frameworks the DLAMI has to offer\. Most users find that the new Deep Learning AMI with Conda is perfect for them\. 

These "Conda" AMIs will be the primary DLAMIs\. It will be updated often with the latest versions from the frameworks, and have the latest GPU drivers and software\. It will be generally referred to as *the* AWS Deep Learning AMI in most documents\.
+ This DLAMI has the following frameworks: Apache MXNet, Caffe, Caffe2, Chainer, CNTK, Keras, PyTorch, TensorFlow and Theano

## Stable versus Bleeding Edge<a name="overview-conda-stability"></a>

The Conda AMIs use optimized binaries of the most recent formal releases from each framework\. Release candidates and experimental features are not to be expected\. The optimizations depend on the framework's support for accelleration technologies like Intel's MKL DNN, which will speed up training and inference on C5, C4, and C3 CPU instance types\. The binaries are also compiled to support advanced Intel instruction sets including, but not limited to AVX, AVX\-2, AVX\-512, SSE4\.1, and SSE4\.2\. These accelerate vector and floating point operations on Intel CPU architectures\. Additionally, for GPU instance types, the CUDA and cuDNN will be updated with whichever version the latest official release supports\. 

 The Deep Learning AMI with Conda automatically installs the most optimized version of the framework for your EC2 instance upon the framework's first activation\. For more information, refer to [Using the Deep Learning AMI with Conda](tutorial-conda.md)\. 

If you want to install from source, using custom or optimized build options, the [Deep Learning Base AMI](overview-base.md)'s might be a better option for you\.

## <a name="overview-conda-cuda"></a>

**CUDA Support**  
The Deep Learning AMI with Conda's CUDA version and the frameworks supported for each:
+ CUDA 9 with cuDNN 7: Apache MXNet, Caffe2, Chainer, CNTK, Keras, PyTorch, TensorFlow, Theano
+ CUDA 8 with cuDNN 6: Caffe

Specific framework version numbers can be found in the [DLAMI: Release Notes](appendix-ami-release-notes.md)

Choose this DLAMI type or learn more about the different DLAMIs with the Next Up option\.
+ [Deep Learning AMI with Conda](conda.md)

**Next Up**  
[Deep Learning Base AMI](overview-base.md)