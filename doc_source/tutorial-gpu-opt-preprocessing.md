# Preprocessing<a name="tutorial-gpu-opt-preprocessing"></a>

Data preprocessing through transformations or augmentations can often be a CPU\-bound process, and this can be the bottleneck in your overall pipeline\. Frameworks have built\-in operators for image processing, but DALI \(Data Augmentation Library\) demonstrates improved performance over frameworks’ built\-in options\.
+ NVIDIA Data Augmentation Library \(DALI\): DALI offloads data augmentation to the GPU\. It is not preinstalled on the DLAMI, but you can access it by installing it or loading a supported framework container on your DLAMI or other Amazon Elastic Compute Cloud instance\. Refer to the [DALI project page](https://docs.nvidia.com/deeplearning/sdk/dali-install-guide/index.html) on the NVIDIA website for details\. For an example use\-case and to download code samples, see the [ SageMaker Preprocessing Training Performance](https://github.com/aws-samples/sagemaker-cv-preprocessing-training-performance) sample\.
+ nvJPEG: a GPU\-accelerated JPEG decoder library for C programmers\. It supports decoding single images or batches as well as subsequent transformation operations that are common in deep learning\. nvJPEG comes built\-in with DALI, or you can download from the [NVIDIA website's nvjpeg page](https://developer.nvidia.com/nvjpeg) and use it separately\.

You might be interested in these other topics on GPU monitoring and optimization:
+ [Monitoring](tutorial-gpu-monitoring.md)
  + [Monitor GPUs with CloudWatch](tutorial-gpu-monitoring-gpumon.md)
+ [Optimization](tutorial-gpu-opt.md)
  + [Preprocessing](#tutorial-gpu-opt-preprocessing)
  + [Training](tutorial-gpu-opt-training.md)