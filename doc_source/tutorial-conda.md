# Using the Deep Learning AMI with Conda<a name="tutorial-conda"></a>

**Topics**
+ [Introduction to the Deep Learning AMI with Conda](#tutorial-conda-overview)
+ [Step 1: Log in to Your DLAMI](#tutorial-conda-login)
+ [Step 2: Start the MXNet Python 3 Environment](#tutorial-conda-switch-mxnet)
+ [Step 3: Test Some MXNet Code](#tutorial-conda-test-mxnet)
+ [Step 4: Switch to the TensorFlow Environment](#tutorial-conda-switch-tf)
+ [Removing Environments](#tutorial-conda-remove-env)

## Introduction to the Deep Learning AMI with Conda<a name="tutorial-conda-overview"></a>

Conda is an open source package management system and environment management system that runs on Windows, macOS, and Linux\. Conda quickly installs, runs, and updates packages and their dependencies\. Conda easily creates, saves, loads and switches between environments on your local computer\.

The Deep Learning AMI with Conda has been configured for you to easily switch between deep learning environments\. The following instructions guide you on some basic commands with `conda`\. They also help you verify that the basic import of the framework is functioning, and that you can run a couple simple operations with the framework\. You can then move on to more thorough tutorials provided with the DLAMI or the frameworks' examples found on each frameworks' project site\.

## Step 1: Log in to Your DLAMI<a name="tutorial-conda-login"></a>

After you log in to your server, you will see a server "message of the day" \(MOTD\) describing various Conda commands that you can use to switch between the different deep learning frameworks\. Below is an example MOTD\. Your specific MOTD may vary as new versions of the DLAMI are released\.

**Note**  
We no longer include the CNTK, Caffe, Caffe2 and Theano Conda environments in the AWS Deep Learning AMI starting with the v28 release\. Previous releases of the AWS Deep Learning AMI that contain these environments will continue to be available\. However, we will only provide updates to these environments if there are security fixes published by the open source community for these frameworks\.

```
=============================================================================
       __|  __|_  )
       _|  (     /   Deep Learning AMI (Ubuntu 16.04) Version 26.0
      ___|\___|___|
=============================================================================

Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-1098-aws x86_64v)

Please use one of the following commands to start the required environment with the framework of your choice:
for MXNet(+Keras2) with Python3 (CUDA 10.1 and Intel MKL-DNN) ____________________________________ source activate mxnet_p36
for MXNet(+Keras2) with Python2 (CUDA 10.1 and Intel MKL-DNN) ____________________________________ source activate mxnet_p27
for MXNet(+Amazon Elastic Inference) with Python3 _______________________________________ source activate amazonei_mxnet_p36
for MXNet(+Amazon Elastic Inference) with Python2 _______________________________________ source activate amazonei_mxnet_p27
for MXNet(+AWS Neuron) with Python3 ___________________________________________________ source activate aws_neuron_mxnet_p36
for TensorFlow(+Keras2) with Python3 (CUDA 10.0 and Intel MKL-DNN) __________________________ source activate tensorflow_p36
for TensorFlow(+Keras2) with Python2 (CUDA 10.0 and Intel MKL-DNN) __________________________ source activate tensorflow_p27
for TensorFlow 2(+Keras2) with Python3 (CUDA 10.0 and Intel MKL-DNN) _______________________ source activate tensorflow2_p36
for TensorFlow 2(+Keras2) with Python2 (CUDA 10.0 and Intel MKL-DNN) _______________________ source activate tensorflow2_p27
for Tensorflow(+Amazon Elastic Inference) with Python2 _____________________________ source activate amazonei_tensorflow_p27
for Tensorflow(+Amazon Elastic Inference) with Python3 _____________________________ source activate amazonei_tensorflow_p36
for Tensorflow(+AWS Neuron) with Python3 _________________________________________ source activate aws_neuron_tensorflow_p36
for PyTorch with Python3 (CUDA 10.1 and Intel MKL) _____________________________________________ source activate pytorch_p36
for PyTorch with Python2 (CUDA 10.1 and Intel MKL) _____________________________________________ source activate pytorch_p27
for Chainer with Python2 (CUDA 10.0 and Intel iDeep) ___________________________________________ source activate chainer_p27
for Chainer with Python3 (CUDA 10.0 and Intel iDeep) ___________________________________________ source activate chainer_p36
for base Python2 (CUDA 10.0) _______________________________________________________________________ source activate python2
for base Python3 (CUDA 10.0) _______________________________________________________________________ source activate python3
```

Each Conda command has the following pattern:

`source activate framework_python-version`

For example, you may see `for MXNet(+Keras1) with Python3 (CUDA 9) _____________________ source activate mxnet_p36`, which signifies that the environment has MXNet, Keras 1, Python 3, and CUDA 9\. And to activate this environment, the command you would use is:

```
$ source activate mxnet_p36
```

The other variation of this would be:

```
$ source activate mxnet_p27
```

This signifies that the environment will have MXNet and Python 2 \(with Keras 1 and CUDA 9\)\.

## Step 2: Start the MXNet Python 3 Environment<a name="tutorial-conda-switch-mxnet"></a>

We will test MXNet first to give you a general idea of how easy it is\.

**Note**  
When you launch your first Conda environment, please be patient while it loads\. The Deep Learning AMI with Conda automatically installs the most optimized version of the framework for your EC2 instance upon the framework's first activation\. You should not expect subsequent delays\.
+ Activate the MXNet virtual environment for Python 3\.

  ```
  $ source activate mxnet_p36
  ```

This activates the environment for MXNet with Python 3\. Alternatively, you could have activated mxnet\_p27 to get an environment with Python 2\.

## Step 3: Test Some MXNet Code<a name="tutorial-conda-test-mxnet"></a>

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

## Step 4: Switch to the TensorFlow Environment<a name="tutorial-conda-switch-tf"></a>

Now we will switch to TensorFlow\! If you're still in the iPython console use `quit()`, then get ready to switch environments\.

1. Activate the TensorFlow virtual environment for Python 3,

   ```
   $ source activate tensorflow_p36
   ```

1. Start the iPython terminal\.

   ```
   (tensorflow_36)$ ipython
   ```

1. Run a quick TensorFlow program\.

   ```
   import tensorflow as tf
   hello = tf.constant('Hello, TensorFlow!')
   sess = tf.Session()
   print(sess.run(hello))
   ```

You should see "Hello, Tensorflow\!" Now you have tested two different deep learning frameworks, and you've seen how to switch between frameworks\.

**Next Up**  
[Running Jupyter Notebook Tutorials](tutorial-jupyter.md)

## Removing Environments<a name="tutorial-conda-remove-env"></a>

If you run out of space on the DLAMI, you can choose to uninstall Conda packages that you are not using:

```
conda env list
conda env remove –-name <env_name>
```