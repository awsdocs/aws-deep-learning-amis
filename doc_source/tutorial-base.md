# Using the Deep Learning Base AMI<a name="tutorial-base"></a>

## Using the Deep Learning Base AMI<a name="tutorial-base-overview"></a>

The Base AMI comes with a foundational platform of GPU drivers and acceleration libraries to deploy your own customized deep learning environment\. By default the AMI is configured with the NVIDIA CUDA 11\.0 environment\. You can also switch between different versions of CUDA\. Refer to the following instructions for how to do this\.

## Configuring CUDA Versions<a name="tutorial-base-cuda"></a>

You can verify the CUDA version by running NVIDIA's `nvcc` program\.

```
nvcc --version
```

You can select and verify a particular CUDA version with the following bash command:

```
sudo rm /usr/local/cuda
sudo ln -s /usr/local/cuda-11.0 /usr/local/cuda
```

For more information, see the [Base DLAMI release notes](https://docs.aws.amazon.com/dlami/latest/devguide/appendix-ami-release-notes.html#appendix-ami-release-notes-base)\.