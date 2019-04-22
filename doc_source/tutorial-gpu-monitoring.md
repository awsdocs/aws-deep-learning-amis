# Monitoring<a name="tutorial-gpu-monitoring"></a>

Your DLAMI comes preinstalled with several GPU monitoring tools\. This guide also mentions tools that are available to download and install\.
+ [Monitor GPUs with CloudWatch](tutorial-gpu-monitoring-gpumon.md) \- a preinstalled utility that reports GPU usage statistics to CloudWatch\.
+ [nvidia\-smi CLI](https://developer.nvidia.com/nvidia-system-management-interface) \- a utility to monitor overall GPU compute and memory utilization\. This is preinstalled on your DLAMI\.
+ [NVML C library](https://developer.nvidia.com/nvidia-management-library-nvml) \- a C\-based API to directly access GPU monitoring and management functions\. This used by the nvidia\-smi CLI under the hood and is preinstalled on your DLAMI\. It also has Python and Perl bindings to facilitate development in those languages\. The gpumon\.py utility preinstalled on your DLAMI uses the pynvml package from [nvidia\-ml\-py](https://pypi.org/project/nvidia-ml-py/)\.
+ [NVIDIA DCGM](https://developer.nvidia.com/data-center-gpu-manager-dcgm) \- A cluster management tool\. Visit the developer page to learn how to install and configure this tool\.

**Tip**  
Check out NVIDIA's developer blog for the latest info on using the CUDA tools installed your DLAMI:  
[Monitoring TensorCore utilization using Nsight IDE and nvprof](https://devblogs.nvidia.com/using-nsight-compute-nvprof-mixed-precision-deep-learning-models/)\.