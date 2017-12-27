# CUDA Installations and Framework Bindings<a name="overview-cuda"></a>

Another consideration is which version of CUDA you require, whether or not the framework supports that version or, if the framework does, is it stable yet\. This stability versus cutting edge choice may not be much of a choice for you\. Deep learning is all pretty cutting edge, however, each framework offers "stable" versions\. These stable versions may not work with the latest CUDA or cuDNN implementation and features\. How do you decide? This ultimately points to your use case and the features you require\. If you don't know, then go with the latest [Deep Learning AMI with Conda](conda.md)\. It has both CUDA 8 and CUDA 9, using whichever most recent version is supported by each framework\. 

Choose one of the CUDA versions and review the full list of DLAMIs that have that version in the Appendix, or learn more about the different DLAMIs with the Next Up option\.

+ [Deep Learning AMI with CUDA 9](cuda9.md)

+ [Deep Learning AMI with CUDA 8](cuda8.md)

**Next Up**  
[DLAMI Operating System Options](overview-os.md)