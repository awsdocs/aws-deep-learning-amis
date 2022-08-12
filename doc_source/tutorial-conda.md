# Using the Deep Learning AMI with Conda<a name="tutorial-conda"></a>

**Topics**
+ [Introduction to the Deep Learning AMI with Conda](#tutorial-conda-overview)
+ [Log in to Your DLAMI](#tutorial-conda-login)
+ [Start the TensorFlow Environment](#tutorial-conda-switch-tf)
+ [Switch to the PyTorch Python 3 Environment](#tutorial-conda-switch-pytorch)
+ [Switch to the MXNet Python 3 Environment](#tutorial-conda-switch-mxnet)
+ [Removing Environments](#tutorial-conda-remove-env)

## Introduction to the Deep Learning AMI with Conda<a name="tutorial-conda-overview"></a>

Conda is an open source package management system and environment management system that runs on Windows, macOS, and Linux\. Conda quickly installs, runs, and updates packages and their dependencies\. Conda easily creates, saves, loads and switches between environments on your local computer\.

The Deep Learning AMI with Conda has been configured for you to easily switch between deep learning environments\. The following instructions guide you on some basic commands with `conda`\. They also help you verify that the basic import of the framework is functioning, and that you can run a couple simple operations with the framework\. You can then move on to more thorough tutorials provided with the DLAMI or the frameworks' examples found on each frameworks' project site\.

## Log in to Your DLAMI<a name="tutorial-conda-login"></a>

After you log in to your server, you will see a server "message of the day" \(MOTD\) describing various Conda commands that you can use to switch between the different deep learning frameworks\. Below is an example MOTD\. Your specific MOTD may vary as new versions of the DLAMI are released\.

**Note**  
We no longer include the CNTK, Caffe, Caffe2, Theano, Chainer, and Keras Conda environments in the AWS Deep Learning AMI starting with the v28 release\. Previous releases of the AWS Deep Learning AMI that contain these environments will continue to be available\. However, we will only provide updates to these environments if there are security fixes published by the open source community for these frameworks\.

```
=============================================================================
       __|  __|_  )
       _|  (     /   Deep Learning AMI (Ubuntu 18.04) Version 40.0
      ___|\___|___|
=============================================================================

Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 5.4.0-1037-aws x86_64v)

Please use one of the following commands to start the required environment with the framework of your choice:
for AWS MX 1.7 (+Keras2) with Python3 (CUDA 10.1 and Intel MKL-DNN) _______________________________ source activate mxnet_p36
for AWS MX 1.8 (+Keras2) with Python3 (CUDA + and Intel MKL-DNN) ___________________________ source activate mxnet_latest_p37
for AWS MX(+AWS Neuron) with Python3 ___________________________________________________ source activate aws_neuron_mxnet_p36
for AWS MX(+Amazon Elastic Inference) with Python3 _______________________________________ source activate amazonei_mxnet_p36
for TensorFlow(+Keras2) with Python3 (CUDA + and Intel MKL-DNN) _____________________________ source activate tensorflow_p37
for Tensorflow(+AWS Neuron) with Python3 _________________________________________ source activate aws_neuron_tensorflow_p36
for TensorFlow 2(+Keras2) with Python3 (CUDA 10.1 and Intel MKL-DNN) _______________________ source activate tensorflow2_p36
for TensorFlow 2.3 with Python3.7 (CUDA + and Intel MKL-DNN) ________________________ source activate tensorflow2_latest_p37
for PyTorch 1.4 with Python3 (CUDA 10.1 and Intel MKL) _________________________________________ source activate pytorch_p36
for PyTorch 1.7.1 with Python3.7 (CUDA 11.0 and Intel MKL) ________________________________ source activate pytorch_latest_p37
for PyTorch (+AWS Neuron) with Python3 ______________________________________________ source activate aws_neuron_pytorch_p36
for base Python3 (CUDA 10.0) _______________________________________________________________________ source activate python3
```

Each Conda command has the following pattern:

`source activate framework_python-version`

For example, you may see `for MXNet(+Keras1) with Python3 (CUDA 10.1) _____________________ source activate mxnet_p36`, which signifies that the environment has MXNet, Keras 1, Python 3, and CUDA 10\.1\. And to activate this environment, the command you would use is:

```
$ source activate mxnet_p36
```

## Start the TensorFlow Environment<a name="tutorial-conda-switch-tf"></a>

**Note**  
When you launch your first Conda environment, please be patient while it loads\. The Deep Learning AMI with Conda automatically installs the most optimized version of the framework for your EC2 instance upon the framework's first activation\. You should not expect subsequent delays\.

1. Activate the TensorFlow virtual environment for Python 3\.

   ```
   $ source activate tensorflow_p37
   ```

1. Start the iPython terminal\.

   ```
   (tensorflow_37)$ ipython
   ```

1. Run a quick TensorFlow program\.

   ```
   import tensorflow as tf
   hello = tf.constant('Hello, TensorFlow!')
   sess = tf.Session()
   print(sess.run(hello))
   ```

You should see "Hello, Tensorflow\!"

**Next Up**  
[Running Jupyter Notebook Tutorials](tutorial-jupyter.md)

## Switch to the PyTorch Python 3 Environment<a name="tutorial-conda-switch-pytorch"></a>

If you're still in the iPython console, use `quit()`, then get ready to switch environments\.
+ Activate the PyTorch virtual environment for Python 3\.

  ```
  $ source activate pytorch_p36
  ```

### Test Some PyTorch Code<a name="tutorial-conda-test-pytorch"></a>

To test your installation, use Python to write PyTorch code that creates and prints an array\.

1. Start the iPython terminal\.

   ```
   (pytorch_p36)$ ipython
   ```

1. Import PyTorch\.

   ```
   import torch
   ```

   You might see a warning message about a third\-party package\. You can ignore it\.

1. Create a 5x3 matrix with the elements initialized randomly\. Print the array\.

   ```
   x = torch.rand(5, 3)
   print(x)
   ```

   Verify the result\.

   ```
   tensor([[0.3105, 0.5983, 0.5410],
           [0.0234, 0.0934, 0.0371],
           [0.9740, 0.1439, 0.3107],
           [0.6461, 0.9035, 0.5715],
           [0.4401, 0.7990, 0.8913]])
   ```

## Switch to the MXNet Python 3 Environment<a name="tutorial-conda-switch-mxnet"></a>

If you're still in the iPython console, use `quit()`, then get ready to switch environments\.
+ Activate the MXNet virtual environment for Python 3\.

  ```
  $ source activate mxnet_p36
  ```

### Test Some MXNet Code<a name="tutorial-conda-test-mxnet"></a>

To test your installation, use Python to write MXNet code that creates and prints an array using the `NDArray` API\. For more information, see [NDArray API](https://mxnet.incubator.apache.org/api/python/ndarray/ndarray.html)\.

1. Start the iPython terminal\.

   ```
   (mxnet_p36)$ ipython
   ```

1. Import MXNet\.

   ```
   import mxnet as mx
   ```

   You might see a warning message about a third\-party package\. You can ignore it\.

1. Create a 5x5 matrix, an instance of the `NDArray`, with elements initialized to 0\. Print the array\.

   ```
   mx.ndarray.zeros((5,5)).asnumpy()
   ```

   Verify the result\.

   ```
   array([[ 0.,  0.,  0.,  0.,  0.],
          [ 0.,  0.,  0.,  0.,  0.],
          [ 0.,  0.,  0.,  0.,  0.],
          [ 0.,  0.,  0.,  0.,  0.],
          [ 0.,  0.,  0.,  0.,  0.]], dtype=float32)
   ```

   You can find more examples of MXNet in the MXNet tutorials section\.

## Removing Environments<a name="tutorial-conda-remove-env"></a>

If you run out of space on the DLAMI, you can choose to uninstall Conda packages that you are not using:

```
conda env list
conda env remove –-name <env_name>
```