# CUDA Installations and Framework Bindings<a name="overview-cuda"></a>

While deep learning is all pretty cutting edge, each framework offers "stable" versions\. These stable versions may not work with the latest CUDA or cuDNN implementation and features\. Your use case and the features you require can help you choose a framework\. If you are not sure, then use the latest Deep Learning AMI with Conda\. It has official `pip` binaries for all frameworks with CUDA 10, using whichever most recent version is supported by each framework\. If you want the latest versions, and to customize your deep learning environment, use the Deep Learning Base AMI\.

Look at our guide on [Stable Versus Release Candidates](overview-conda.md#overview-conda-stability) for further guidance\.

## Choose a DLAMI with CUDA<a name="cuda-choose"></a>

The [Deep Learning Base AMI](overview-base.md) has all available CUDA 11 series, including 11\.0, 11\.1, and 11\.2\.

The [Deep Learning AMI with Conda](overview-conda.md) has all available CUDA 11 series, including 11\.0, 11\.1, and 11\.2\.

**Note**  
We no longer include the CNTK, Caffe, Caffe2, Theano, Chainer, or Keras Conda environments in the AWS Deep Learning AMI starting with the v28 release\. Previous releases of the AWS Deep Learning AMI that contain these environments continue to be available\. However, we only provide updates to these environments if there are security fixes published by the open\-source community for these frameworks\.

For specific framework version numbers, see the [Release Notes for DLAMI](appendix-ami-release-notes.md)

Choose this DLAMI type or learn more about the different DLAMIs with the **Next Up** option\.

Choose one of the CUDA versions and review the full list of DLAMIs that have that version in the **Appendix**, or learn more about the different DLAMIs with the **Next Up** option\.

**Next Up**  
[Deep Learning Base AMI](overview-base.md)

## Related Topics<a name="cuda-related"></a>
+ For instructions on switching between CUDA versions, refer to the [Using the Deep Learning Base AMI](tutorial-base.md) tutorial\.