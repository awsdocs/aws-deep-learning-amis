# Deep Learning AMI with Source Code<a name="overview-source"></a>

**Important**  
**These DLAMI are no longer being updated\. It is advised to use the [Deep Learning AMI with Conda](overview-conda.md) or [Deep Learning Base AMI](overview-base.md)**\.

Deep Learning AMI with Source Code refers to DLAMIs that use one Python environment for all of the frameworks\. Technically, there are two environments, Python 2\.7 and Python 3\.5, and the frameworks are installed once into each of those Python versions\. 

These AMIs contain bleeding\-edge builds of frameworks built directly from source, with advanced optimizations not found in stock pip packages offered by frameworks\. It also has the source code for the frameworks, making it a larger disk image\. It is often used by someone who plans to modify the source code directly, or contributors to the frameworks' open source projects\.
+ Deep Learning AMI with Source Code \(CUDA 9\) is built with: Apache MXNet, Caffe2, Keras, PyTorch, and TensorFlow
+ Deep Learning AMI with Source Code \(CUDA 8\) is built with: Apache MXNet, Caffe, Caffe2, CNTK, Keras, TensorFlow, Theano, and Torch

Specific framework version numbers can be found in the [DLAMI: Release Notes](appendix-ami-release-notes.md)

Choose this DLAMI type or learn more about the different DLAMIs with the Next Up option\.
+ [Deep Learning AMI with Source Code](source.md)

**Next Up**  
[CUDA Installations and Framework Bindings](overview-cuda.md)