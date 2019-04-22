# Training<a name="tutorial-gpu-opt-training"></a>

With mixed\-precision training you can deploy larger networks with the same amount of memory, or reduce memory usage compared to your single or double precision network, and you will see compute performance increases\. You also get the benefit of smaller and faster data transfers, an important factor in multiple node distributed training\. To take advantage of mixed\-precision training you need to adjust data casting and loss scaling\. The following are guides describing how to do this for the frameworks that support mixed\-precision\.
+ [NVIDIA Deep Learning SDK](https://docs.nvidia.com/deeplearning/sdk/mixed-precision-training/) \- docs on the NVIDIA website describing mixed\-precision implementation for NVCaffe, Caffe2, CNTK, MXNet, PyTorch, TensorFlow and Theano\.

**Tip**  
Be sure to check the website for your framework of choice, and search for "mixed precision" or "fp16" for the latest optimization techniques\. Here are some mixed\-precision guides you might find helpful:  
[Mixed\-precision training with TensorFlow \(video\)](https://devblogs.nvidia.com/mixed-precision-resnet-50-tensor-cores/) \- on the NVIDIA blog site\.
[Mixed\-precision training using float16 with MXNet](https://mxnet.incubator.apache.org/versions/master/faq/float16.html) \- an FAQ article on the MXNet website\.
[NVIDIA Apex: a tool for easy mixed\-precision training with PyTorch](https://devblogs.nvidia.com/apex-pytorch-easy-mixed-precision-training/) \- a blog article on the NVIDIA website\.

You might be interested in these other topics on GPU monitoring and optimization:
+ [Monitoring](tutorial-gpu-monitoring.md)
  + [Monitor GPUs with CloudWatch](tutorial-gpu-monitoring-gpumon.md)
+ [Optimization](tutorial-gpu-opt.md)
  + [Preprocessing](tutorial-gpu-opt-preprocessing.md)
  + [Training](#tutorial-gpu-opt-training)