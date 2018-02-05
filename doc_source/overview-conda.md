# Deep Learning AMI with Conda<a name="overview-conda"></a>

The newest DLAMI uses Anaconda virtual environments\. These environments are configured to keep the different framework installations separate\. It also makes it easy to switch between frameworks\. This is great for learning and experimenting with all of the frameworks the DLAMI has to offer\. Most users find that the new Deep Learning AMI with Conda is perfect for them\. 

These "Conda" AMIs will be the primary DLAMIs\. It will be updated often with the latest versions from the frameworks, and have the latest GPU drivers and software\. It will be generally referred to as *the* AWS Deep Learning AMI in most documents\.

+ This DLAMI has the following frameworks: Apache MXNet, Caffe, Caffe2, CNTK, Keras, PyTorch, TensorFlow and Theano

## Stable versus Bleeding Edge<a name="overview-conda-stability"></a>

The Conda AMIs use the most recent formal releases from each framework using the pip packages on PyPI\. Release candidates and experimental features are not to be expected\. The CUDA and cuDNN versions will match whatever the official release supports\. If you are interested in bleeding edge builds of the frameworks built directly from source, you may want the [Deep Learning AMI with CUDA 9](cuda9.md) that support CUDA 9 and cuDNN 7\. Or, if you want to install from source, using custom or optimized build options, the [Deep Learning Base AMI](overview-base.md)'s might be a better option for you\.

**CUDA Support**  
The Deep Learning AMI with Conda's CUDA version and the frameworks supported for each \(Keras support depends on each framework\):

+ CUDA 9: Apache MXNet, Caffe2, PyTorch, Theano

+ CUDA 8: CNTK, TensorFlow, and Caffe

Specific framework version numbers can be found in the [DLAMI: Release Notes](appendix-ami-release-notes.md)

Choose this DLAMI type or learn more about the different DLAMIs with the Next Up option\.

+ [Deep Learning AMI with Conda](conda.md)

**Next Up**  
[Deep Learning AMI with Source Code](overview-source.md)