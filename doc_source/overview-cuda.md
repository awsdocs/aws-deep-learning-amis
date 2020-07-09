# CUDA Installations and Framework Bindings<a name="overview-cuda"></a>

Deep learning is all pretty cutting edge, however, each framework offers "stable" versions\. These stable versions may not work with the latest CUDA or cuDNN implementation and features\. How do you decide? This ultimately points to your use case and the features you require\. If you are not sure, then go with the latest Deep Learning AMI with Conda\. It has official pip binaries of all frameworks with CUDA 8, CUDA 9, CUDA 10, and CUDA 10\.1 using whichever most recent version is supported by each framework\. If you want the latest versions, and to customize your deep learning environment, go with the Deep Learning Base AMI\.

Look at our guide on [Stable versus Release Candidates](overview-conda.md#overview-conda-stability) for further guidance\.

## Choosing a DLAMI with CUDA<a name="w19aab7b5c17b7"></a>

The [Deep Learning Base AMI](overview-base.md) has CUDA 8, 9, 10, and 10\.1\.

The [Deep Learning AMI with Conda](overview-conda.md) has CUDA 8 and 9, 10, and 10\.1\.
+ CUDA 10\.1 with cuDNN 7: Apache MXNet
+ CUDA 10 with cuDNN 7: PyTorch, TensorFlow, TensorFlow 2, Apache MXNet, Chainer

**Note**  
We no longer include the CNTK, Caffe, Caffe2 and Theano Conda environments in the AWS Deep Learning AMI starting with the v28 release\. Previous releases of the AWS Deep Learning AMI that contain these environments will continue to be available\. However, we will only provide updates to these environments if there are security fixes published by the open source community for these frameworks\.

For installation options for DLAMI types and operating systems, refer to each of the CUDA version and options pages:
+ [Deep Learning AMI with CUDA 10\.1 Options](cuda10-1.md)
+ [Deep Learning AMI with CUDA 10 Options](cuda10.md)
+ [Deep Learning AMI with CUDA 9 Options](cuda9.md)
+ [Deep Learning AMI with CUDA 8 Options](cuda8.md)
+ [Deep Learning AMI with Conda Options](conda.md)
+ [Deep Learning Base AMI Options](base.md)

Specific framework version numbers can be found in the [Release Notes for DLAMI](appendix-ami-release-notes.md)

Choose this DLAMI type or learn more about the different DLAMIs with the Next Up option\.

Choose one of the CUDA versions and review the full list of DLAMIs that have that version in the Appendix, or learn more about the different DLAMIs with the Next Up option\.

**Next Up**  
[DLAMI Operating System Options](overview-os.md)

## Related Topics<a name="w19aab7b5c17b9"></a>
+ For instructions on switching between CUDA versions, refer to the [Using the Deep Learning Base AMI](tutorial-base.md) tutorial\.