# Using the Graviton GPU DLAMI<a name="tutorial-graviton-cuda"></a>

The AWS Deep Learning AMI is ready to use with Arm processor\-based Graviton GPUs\. The Graviton GPU DLAMI comes with a foundational platform of GPU drivers and acceleration libraries to deploy your own customized deep learning environment\. Docker and NVIDIA Docker are preconfigured on the Graviton GPU DLAMI to let you deploy containerzied applications\. Check the [release notes](http://aws.amazon.com/releasenotes/aws-deep-learning-ami-graviton-gpu-cuda-11-4-ubuntu-20-04/) for additional details on the Graviton GPU DLAMI\.

**Topics**
+ [Check GPU Status](#tutorial-graviton-cuda-check)
+ [Check CUDA Version](#tutorial-graviton-cuda-version)
+ [Verify Docker](#tutorial-graviton-cuda-docker)
+ [TensorRT](#tutorial-graviton-cuda-tensorrt)
+ [Run CUDA Samples](#tutorial-graviton-cuda-samples)

## Check GPU Status<a name="tutorial-graviton-cuda-check"></a>

Use the [NVIDIA System Management Interface](https://developer.nvidia.com/nvidia-system-management-interface) to check the status of your Graviton GPU\.

```
nvidia-smi
```

The output of the `nvidia-smi` command should be similar to the following:

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 470.82.01    Driver Version: 470.82.01    CUDA Version: 11.4     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA T4G          On   | 00000000:00:1F.0 Off |                    0 |
| N/A   32C    P8     8W /  70W |      0MiB / 15109MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```

## Check CUDA Version<a name="tutorial-graviton-cuda-version"></a>

Run the following command to check your CUDA version:

```
/usr/local/cuda/bin/nvcc --version | grep Cuda
```

Your output should look similar to the following:

```
nvcc: NVIDIA (R) Cuda compiler driver
Cuda compilation tools, release 11.4, V11.4.120
```

## Verify Docker<a name="tutorial-graviton-cuda-docker"></a>

Run a CUDA container from [DockerHub](https://hub.docker.com/r/nvidia/cuda) to verify Docker functionality on your Graviton GPU:

```
sudo docker run --platform=linux/arm64 --rm \
    --gpus all nvidia/cuda:11.4.2-base-ubuntu20.04 nvidia-smi
```

Your output should look similar to the following:

```
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 470.82.01    Driver Version: 470.82.01    CUDA Version: 11.4     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  NVIDIA T4G          On   | 00000000:00:1F.0 Off |                    0 |
| N/A   33C    P8     9W /  70W |      0MiB / 15109MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                                  |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```

## TensorRT<a name="tutorial-graviton-cuda-tensorrt"></a>

Use the following command to access the TensorRT command line tool:

```
trtexec
```

Your output should look similar to the following:

```
&&&& RUNNING TensorRT.trtexec [TensorRT v8200] # trtexec
...
&&&& PASSED TensorRT.trtexec [TensorRT v8200] # trtexec
```

There are TensorRT Python wheels available for installation on demand\. You can find these wheels in the following file locations:

```
/usr/local/tensorrt/graphsurgeon/
└── graphsurgeon-0.4.5-py2.py3-none-any.whl

/usr/local/tensorrt/onnx_graphsurgeon/
└── onnx_graphsurgeon-0.3.12-py2.py3-none-any.whl

/usr/local/tensorrt/python/
├── tensorrt-8.2.0.6-cp36-none-linux_aarch64.whl
├── tensorrt-8.2.0.6-cp37-none-linux_aarch64.whl
├── tensorrt-8.2.0.6-cp38-none-linux_aarch64.whl
└── tensorrt-8.2.0.6-cp39-none-linux_aarch64.whl

/usr/local/tensorrt/uff/
└── uff-0.6.9-py2.py3-none-any.whl
```

For additional details, see the [NVIDIA TensorRT documentation](https://docs.nvidia.com/deeplearning/tensorrt/developer-guide/index.html)\.

## Run CUDA Samples<a name="tutorial-graviton-cuda-samples"></a>

The Graviton GPU DLAMI provides pre\-compiled CUDA samples to help you verify different CUDA functionalities\.

```
ls /usr/local/cuda/compiled_samples
```

For example, run the `vectorAdd` sample with the following command:

```
/usr/local/cuda/compiled_samples/vectorAdd
```

Your output should look similar to the following:

```
[Vector addition of 50000 elements]
Copy input data from the host memory to the CUDA device
CUDA kernel launch with 196 blocks of 256 threads
Copy output data from the CUDA device to the host memory
Test PASSED
Done
```

Run the `transpose` sample:

```
/usr/local/cuda/compiled_samples/transpose
```

Your output should look similar to the following:

```
Transpose Starting...

GPU Device 0: "Turing" with compute capability 7.5

> Device 0: "NVIDIA T4G"
> SM Capability 7.5 detected:
> [NVIDIA T4G] has 40 MP(s) x 64 (Cores/MP) = 2560 (Cores)
> Compute performance scaling factor = 1.00

Matrix size: 1024x1024 (64x64 tiles), tile size: 16x16, block size: 16x16

transpose simple copy       , Throughput = 185.1781 GB/s, Time = 0.04219 ms, Size = 1048576 fp32 elements, NumDevsUsed = 1, Workgroup = 256
transpose shared memory copy, Throughput = 163.8616 GB/s, Time = 0.04768 ms, Size = 1048576 fp32 elements, NumDevsUsed = 1, Workgroup = 256
transpose naive             , Throughput = 98.2805 GB/s, Time = 0.07949 ms, Size = 1048576 fp32 elements, NumDevsUsed = 1, Workgroup = 256
transpose coalesced         , Throughput = 127.6759 GB/s, Time = 0.06119 ms, Size = 1048576 fp32 elements, NumDevsUsed = 1, Workgroup = 256
transpose optimized         , Throughput = 156.2960 GB/s, Time = 0.04999 ms, Size = 1048576 fp32 elements, NumDevsUsed = 1, Workgroup = 256
transpose coarse-grained    , Throughput = 155.9157 GB/s, Time = 0.05011 ms, Size = 1048576 fp32 elements, NumDevsUsed = 1, Workgroup = 256
transpose fine-grained      , Throughput = 158.4177 GB/s, Time = 0.04932 ms, Size = 1048576 fp32 elements, NumDevsUsed = 1, Workgroup = 256
transpose diagonal          , Throughput = 133.4277 GB/s, Time = 0.05855 ms, Size = 1048576 fp32 elements, NumDevsUsed = 1, Workgroup = 256
Test passed
```

**Next Up**  
[Using the Graviton GPU TensorFlow DLAMI](tutorial-graviton-tensorflow.md)