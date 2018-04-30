# Using the Deep Learning Base AMI<a name="tutorial-base"></a>

## Using to the Deep Learning Base AMI<a name="tutorial-base-overview"></a>

The Base AMI comes with a foundational platform of GPU drivers and acceleration libraries to deploy your own customized deep learning environment\. By default the AMI is configured with NVidia CUDA 9 environment\. However, you can also switch to a CUDA 8 environment by reconfiguring the environment variable `LD_LIBRARY_PATH`\. You simply need to replace the CUDA 9 portion of the environment variable string with its CUDA 8 equivalent\.

## Configuring CUDA Versions<a name="tutorial-base-cuda"></a>

CUDA 9 portion of the LD\_LIBRARY\_PATH string \(installed by default\)

**\.\.\. *:/usr/local/cuda\-9\.0/lib64:/usr/local/cuda\-9\.0/extras/CUPTI/lib64:/lib/nccl/cuda\-9* :\.\.\.rest of LD\_LIBRARY\_PATH value**

Replace with CUDA 8

**\.\.\. *:/usr/local/cuda\-8\.0/lib64:/usr/local/cuda\-8\.0/extras/CUPTI/lib64:/lib/nccl/cuda\-8* :\.\.\.rest of LD\_LIBRARY\_PATH value**

**Tip**  
Refer to the release notes for information regarding known issues:  
[Deep Learning Base AMI \(Amazon Linux\) Known IssuesDeep Learning Base AMI \(Ubuntu\) Known Issues](BASE_AML1.md#BASE_AML1-known-issues)