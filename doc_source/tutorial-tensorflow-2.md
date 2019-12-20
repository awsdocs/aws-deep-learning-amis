# TensorFlow 2<a name="tutorial-tensorflow-2"></a>

This tutorial shows how to activate TensorFlow 2 on an instance running the Deep Learning AMI with Conda \(DLAMI on Conda\) and run a TensorFlow 2 program\.

When a stable Conda package of a framework is released, it's tested and pre\-installed on the DLAMI\. If you want to run the latest, untested nightly build, you can [Install TensorFlow 2's Nightly Build \(experimental\)](#tutorial-tensorflow-2-install) manually\. 

## Activating TensorFlow 2<a name="tutorial-tensorflow-2-overview"></a>

**To run TensorFlow on the DLAMI with Conda**

1. To activate TensorFlow 2, open an Amazon Elastic Compute Cloud \(Amazon EC2\) instance of the DLAMI with Conda\.
   + For TensorFlow 2 and Keras 2 on Python 3 with CUDA 10\.0 and MKL\-DNN, run this command:

     ```
     $ source activate tensorflow2_p36
     ```
   + For TensorFlow 2 and Keras 2 on Python 2 with CUDA 10\.0 and MKL\-DNN, run this command:

     ```
     $ source activate tensorflow2_p27
     ```

1. Start the iPython terminal:

   ```
   (tensorflow2_p36)$ ipython
   ```

1. Run a TensorFlow 2 program to verify that it is working properly:

   ```
   import tensorflow as tf
   hello = tf.constant('Hello, TensorFlow!')
   tf.print(hello)
   ```

   `Hello, TensorFlow!` should appear on your screen\.

## Install TensorFlow 2's Nightly Build \(experimental\)<a name="tutorial-tensorflow-2-install"></a>

You can install the latest TensorFlow 2 build into either or both of the TensorFlow 2 Conda environments on your Deep Learning AMI with Conda\.

**To install TensorFlow from a nightly build**

1. 
   + For the Python 3 TensorFlow 2 environment, run the following command:

     ```
     $ source activate tensorflow2_p36
     ```
   + For the Python 2 TensorFlow 2 environment, run the following command:

     ```
     $ source activate tensorflow2_p27
     ```

1. Remove the currently installed TensorFlow\.
**Note**  
The remaining steps assume you are using the `tensorflow2_p36` environment\.

   ```
   (tensorflow2_p36)$ pip uninstall tensorflow
   ```

1. Install the latest nightly build of TensorFlow\.

   ```
   (tensorflow2_p36)$ pip install tf-nightly
   ```

1. To verify you have successfully installed latest nightly build, start the IPython terminal and check the version of TensorFlow\.

   ```
   (tensorflow2_p36)$ ipython
   ```

   ```
   import tensorflow
   print (tensorflow.__version__)
   ```

   The output should print something similar to `2.1.0-dev20191122`

## More Tutorials<a name="tutorial-tensorflow-2-more"></a>

For tutorials, see the folder called `Deep Learning AMI with Conda tutorials` in the home directory of the DLAMI\. 

For more tutorials and examples, see the TensorFlow documentation for the [TensorFlow Python API](https://www.tensorflow.org/api_docs/python/) or see the [TensorFlow](https://www.tensorflow.org) website\.