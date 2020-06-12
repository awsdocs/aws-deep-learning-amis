# Using the Deep Learning Base AMI<a name="tutorial-base"></a>

## Using the Deep Learning Base AMI<a name="tutorial-base-overview"></a>

The Base AMI comes with a foundational platform of GPU drivers and acceleration libraries to deploy your own customized deep learning environment\. By default the AMI is configured with the NVIDIA CUDA 10 environment\. However, you can also switch to a CUDA 9 or CUDA 8 environment\. Refer to the following instructions for how to do this\.

## Configuring CUDA Versions<a name="tutorial-base-cuda"></a>

You can select and verify a particular CUDA version with the following bash commands\. 
+ CUDA 10\.2:

  ```
  $ sudo rm /usr/local/cuda
  sudo ln -s /usr/local/cuda-10.2 /usr/local/cuda
  ```
+ CUDA 10\.1:

  ```
  $ sudo rm /usr/local/cuda
  sudo ln -s /usr/local/cuda-10.1 /usr/local/cuda
  ```
+ CUDA 10:

  ```
  $ sudo rm /usr/local/cuda
  sudo ln -s /usr/local/cuda-10.0 /usr/local/cuda
  ```
+ CUDA 9\.2:

  ```
  $ sudo rm /usr/local/cuda
  sudo ln -s /usr/local/cuda-9.2 /usr/local/cuda
  ```
+ CUDA 9:

  ```
  $ sudo rm /usr/local/cuda
  sudo ln -s /usr/local/cuda-9.0 /usr/local/cuda
  ```
+ CUDA 8:

  ```
  $ sudo rm /usr/local/cuda
  sudo ln -s /usr/local/cuda-8.0 /usr/local/cuda
  ```

**Tip**  
You can then verify the CUDA version by running NVIDIA's `deviceQuery` program\.  

```
$ cd /usr/local/cuda/samples/1_Utilities/deviceQuery
sudo make
./deviceQuery
```