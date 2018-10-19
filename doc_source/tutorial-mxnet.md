# Apache MXNet \(incubating\)<a name="tutorial-mxnet"></a>

## Activating MXNet<a name="tutorial-mxnet-overview"></a>

This tutorial shows how to activate MXNet on an instance running the Deep Learning AMI with Conda \(DLAMI on Conda\) and run a MXNet program\.

When a stable Conda package of a framework is released, it's tested and pre\-installed on the DLAMI\. If you want to run the latest, untested nightly build, you can [Installing MXNet's Nightly Build \(experimental\)](#tutorial-mxnet-install) manually\. 

**To run MXNet on the DLAMI with Conda**

1. To activate the framework, open an Amazon Elastic Compute Cloud \(Amazon EC2\) instance of the DLAMI with Conda\.
   + For MXNet and Keras 2 on Python 3 with CUDA 9\.0 and MKL\-DNN, run this command:

     ```
     $ source activate mxnet_p36
     ```
   + For MXNet and Keras 2 on Python 2 with CUDA 9\.0 and MKL\-DNN, run this command:

     ```
     $ source activate mxnet_p27
     ```

1. Start the iPython terminal\.

   ```
   (mxnet_p36)$ ipython
   ```

1. Run a quick MXNet program\. Create a 5x5 matrix, an instance of the `NDArray`, with elements initialized to 0\. Print the array\.

   ```
   import mxnet as mx
   mx.ndarray.zeros((5,5)).asnumpy()
   ```

1. Verify the result\.

   ```
   array([[ 0.,  0.,  0.,  0.,  0.],
          [ 0.,  0.,  0.,  0.,  0.],
          [ 0.,  0.,  0.,  0.,  0.],
          [ 0.,  0.,  0.,  0.,  0.],
          [ 0.,  0.,  0.,  0.,  0.]], dtype=float32)
   ```

## Installing MXNet's Nightly Build \(experimental\)<a name="tutorial-mxnet-install"></a>

You can install the latest MXNet build into either or both of the MXNet Conda environments on your Deep Learning AMI with Conda\.

**To install MXNet from a nightly build**

1. 
   + For the Python 3 MXNet environment, run this command:

     ```
     $ source activate mxnet_p36
     ```
   + For the Python 2 MXNet environment, run this command:

     ```
     $ source activate mxnet_p27
     ```

1. Remove the currently installed MXNet\.
**Note**  
The remaining steps assume you are using the `mxnet_p36` environment\.

   ```
   (mxnet_p36)$ pip uninstall mxnet-cu90mkl
   ```

1. Install the latest nightly build of MXNet\.

   ```
   (mxnet_p36)$ pip install --pre mxnet-cu90mkl
   ```

1. To verify you have successfully installed latest nightly build, start the IPython terminal and check the version of MXNet\.

   ```
   (mxnet_p36)$ ipython
   ```

   ```
   import mxnet
   print (mxnet.__version__)
   ```

   The output should print something similar to `1.3.1`

## More Tutorials<a name="tutorial-mxnet-more"></a>

You can find more tutorials in the Deep Learning AMI with Conda tutorials folder, which is in the home directory of the DLAMI\. Your Deep Learning AMI with Conda also comes with 

1. [Model Server for Apache MXNet \(MMS\)](tutorial-mms.md)

For more tutorials and examples, see the framework's official Python documentation, the [Python API for MXNet](https://mxnet.incubator.apache.org/api/python/index.html), or the [Apache MXNet](https://mxnet.incubator.apache.org/) website\.