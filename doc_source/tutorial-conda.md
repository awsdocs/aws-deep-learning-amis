# Using the Deep Learning AMI with Conda<a name="tutorial-conda"></a>


+ [Introduction to the Deep Learning AMI with Conda](#tutorial-conda-overview)
+ [Step 1: Log in to Your DLAMI](#tutorial-conda-login)
+ [Step 2: Start the MXNet Python 3 Environment](#tutorial-conda-switch-mxnet)
+ [Step 3: Test Some MXNet Code](#tutorial-conda-test-mxnet)
+ [Step 4: Switch to the TensorFlow Environment](#tutorial-conda-switch-tf)

## Introduction to the Deep Learning AMI with Conda<a name="tutorial-conda-overview"></a>

Conda is an open source package management system and environment management system that runs on Windows, macOS, and Linux\. Conda quickly installs, runs, and updates packages and their dependencies\. Conda easily creates, saves, loads and switches between environments on your local computer\.

The Deep Learning AMI with Conda has been configured for you to easily switch between deep learning environments\. The following instructions guide you on some basic commands with `conda`\. They also help you verify that the basic import of the framework is functioning, and that you can run a couple simple operations with the framework\. You can then move on to more thorough tutorials provided with the DLAMI or the frameworks' examples found on each frameworks' project site\.

## Step 1: Log in to Your DLAMI<a name="tutorial-conda-login"></a>

After you log in to your server, you will see a server "message of the day" \(MOTD\) describing various conda commands that you can use to switch between the different deep learning frameworks\.

```
=============================================================================
       __|  __|_  )
       _|  (     /   Deep Learning AMI  (Ubuntu)
      ___|\___|___|
=============================================================================

Welcome to Ubuntu 16.04.3 LTS (GNU/Linux 4.4.0-1039-aws x86_64v)

Please use one of the following commands to start the required environment with the framework of your choice:
for MXNet(+Keras1) with Python3 (CUDA 9) _____________________ source activate mxnet_p36
for MXNet(+Keras1) with Python2 (CUDA 9) _____________________ source activate mxnet_p27
for TensorFlow(+Keras2) with Python3 (CUDA 8) ________________ source activate tensorflow_p36
for TensorFlow(+Keras2) with Python2 (CUDA 8) ________________ source activate tensorflow_p27
for Theano(+Keras2) with Python3 (CUDA 9) ____________________ source activate theano_p36
for Theano(+Keras2) with Python2 (CUDA 9) ____________________ source activate theano_p27
for PyTorch with Python3 (CUDA 8) ____________________________ source activate pytorch_p36
for PyTorch with Python2 (CUDA 8) ____________________________ source activate pytorch_p27
for CNTK(+Keras2) with Python3 (CUDA 8) ______________________ source activate cntk_p36
for CNTK(+Keras2) with Python2 (CUDA 8) ______________________ source activate cntk_p27
for Caffe2 with Python2 (CUDA 9) _____________________________ source activate caffe2_p27
for base Python2 (CUDA 9) ____________________________________ source activate python2
for base Python3 (CUDA 9) ____________________________________ source activate python3
```

Each conda command has the following pattern:

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

+ Activate the MXNet virtual environment for Python 3\.

  ```
  $ source activate mxnet_p36
  ```

This activates the environment for MXNet with Python 3\. Alternatively, you could have activated mxnet\_p27 to get an environment with Python 2\.

## Step 3: Test Some MXNet Code<a name="tutorial-conda-test-mxnet"></a>

To test your installation, use Python to write MXNet code that creates and prints an array using the `NDArray` API\. For more information, see [NDArray API](http://mxnet.io/api/python/ndarray.html)\.

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

**Tip**  
Refer to the release notes for information regarding known issues:  
[Deep Learning AMI \(Ubuntu\) Known Issues](CONDA_UBUNTU1.md#CONDA_UBUNTU1-known-issues)
[Deep Learning AMI \(Amazon Linux\) Known Issues](CONDA_AML1.md#CONDA_AML1-known-issues)

**Next Up**  
[Running Jupyter Notebook Tutorials](tutorial-jupyter.md)