# Document History for AWS Deep Learning AMI Developer Guide<a name="doc-history"></a>

| Change | Description | Date | 
| --- |--- |--- |
| [Removed support for CUDA 8 and 9](overview-cuda.md) | The Deep Learning Base AMI and Deep Learning AMI with Conda no longer support CUDA 8 or 9\. | August 19, 2020 | 
| [TensorFlow 2](tutorial-tensorflow-2.md) |  The Deep Learning AMI with Conda now comes with TensorFlow 2 with CUDA 10\. | December 3, 2019 | 
| [AWS Inferentia](tutorial-inferentia.md) |  The Deep Learning AMI now supports AWS Inferentia hardware and the AWS Neuron SDK\. | December 3, 2019 | 
| [PyTorch 1\.0 with CUDA 10](overview-conda.md#overview-conda-cuda) | The Deep Learning AMI with Conda now comes with PyTorch 1\.0 with CUDA 10\. | December 13, 2018 | 
| [CUDA 10 available on the Deep Learning Base AMI](overview-cuda.md) | CUDA 10 was added as on option for the Deep Learning Base AMI\. Instructions on how to change and test CUDA versions\. | December 11, 2018 | 
| [Using TensorFlow Serving with an Inception Model](tutorial-tfserving.md) | An example for using inference with an Inception model was added for TensorFlow Serving, for both with and without Elastic Inference\. | November 28, 2018 | 
| [Training with 256 GPUs with TensorFlow and Horovod](tutorial-horovod-tensorflow.md) | The TensorFlow with Horovod tutorial was updated to add an example of multiple\-node training\. | November 28, 2018 | 
| [Elastic Inference](launch-config.md) | Elastic inference prerequisites and related info was added to the setup guide\. | November 28, 2018 | 
| [MMS v1\.0 released on the DLAMI\.](tutorial-mms.md) | The MMS tutorial was updated to use the new model archive format \(\.mar\) and demonstrates the new start and stop features\. | November 15, 2018 | 
| [Installing TensorFlow from a Nightly Build](tutorial-tensorflow.md) | A tutorial was added that covers how you can uninstall TensorFlow, then install a nightly build of TensorFlow on your Deep Learning AMI with Conda\. | October 16, 2018 | 
| [Installing CNTK from a Nightly Build](tutorial-cntk.md) | A tutorial was added that covers how you can uninstall CNTK, then install a nightly build of CNTK on your Deep Learning AMI with Conda\. | October 16, 2018 | 
| [Installing Apache MXNet \(Incubating\) from a Nightly Build](tutorial-mxnet.md) | A tutorial was added that covers how you can uninstall MXNet, then install a nightly build of MXNet on your Deep Learning AMI with Conda\. | October 16, 2018 | 
| [Installing PyTorch from a Nightly Build](tutorial-pytorch.md) | A tutorial was added that covers how you can uninstall PyTorch, then install a nightly build of PyTorch on your Deep Learning AMI with Conda\. | September 25, 2018 | 
| [Docker is now pre\-installed on your DLAMI](resources.md#faq) | Since v14 of the Deep Learning AMI with Conda, Docker and NVIDIA's version of Docker for GPUs has been pre\-installed\. | September 25, 2018 | 
| [TensorBoard Tutorial](tutorial-tensorboard.md#tutorial-tensorboard-example) | Example was moved to \~/examples/tensorboard\. Tutorial paths updated\. | July 23, 2018 | 
| [MXBoard Tutorial](debugging-and-visualization.md) | A tutorial on how to use MXBoard for visualization of MXNet models was added\. | July 23, 2018 | 
| [Distributed Training Tutorials](distributed-training.md) | A tutorial on how to use Keras\-MXNet for multi\-GPU training was added\. Chainer's tutorial was updated to for v4\.2\.0\. | July 23, 2018 | 
| [Conda Tutorial](tutorial-conda.md#tutorial-conda-login) | The example MOTD was updated to reflect a more recent release\. | July 23, 2018 | 
| [Chainer Tutorial](tutorial-chainer.md#tutorial-chainer-multi-gpu) | The tutorial was updated to use the latest examples from Chainer's source\. | July 23, 2018 | 

*Earlier Updates:*

The following table describes important changes in each release of the AWS Deep Learning AMI before July, 2018\.


****  

| Change | Description | Date | 
| --- | --- | --- | 
| TensorFlow with Horovod | Added a tutorial for training ImageNet with TensorFlow and Horovod\.  | June 6, 2018 | 
| Upgrading guide | Added the upgrading guide\. | May 15, 2018 | 
| New regions and new 10 minute tutorial | New regions added: US West \(N\. California\), South America, Canada \(Central\), EU \(London\), and EU \(Paris\)\. Also, the first release of a 10\-minute tutorial titled: "Getting Started with Deep Learning AMI"\. | April 26, 2018 | 
| Chainer tutorial | A tutorial for using Chainer in multi\-GPU, single GPU, and CPU modes was added\. CUDA integration was upgraded from CUDA 8 to CUDA 9 for several frameworks\. | February 28, 2018 | 
| Linux AMIs v3\.0, plus introduction of MXNet Model Server, TensorFlow Serving, and TensorBoard | Added tutorials for Conda AMIs with new model and visualization serving capabilities using MXNet Model Server v0\.1\.5, TensorFlow Serving v1\.4\.0, and TensorBoard v0\.4\.0\. AMI and framework CUDA capabilities described in Conda and CUDA overviews\. Latest release notes moved to [https://aws.amazon.com/releasenotes/](https://aws.amazon.com/releasenotes/) | January 25, 2018 | 
| Linux AMIs v2\.0 | Base, Source, and Conda AMIs updated with NCCL 2\.1\. Source and Conda AMIs updated with MXNet v1\.0, PyTorch 0\.3\.0, and Keras 2\.0\.9\. | December 11, 2017 | 
| Two Windows AMI options added | Windows 2012 R2 and 2016 AMIs released: added to AMI selection guide and added to release notes\. | November 30, 2017 | 
| Initial documentation release | Detailed description of change with link to topic/section that was changed\. | November 15, 2017 | 