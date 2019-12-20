# TensorFlow<a name="tutorial-tensorflow"></a>

## Activating TensorFlow<a name="tutorial-tensorflow-overview"></a>

This tutorial shows how to activate TensorFlow on an instance running the Deep Learning AMI with Conda \(DLAMI on Conda\) and run a TensorFlow program\.

When a stable Conda package of a framework is released, it's tested and pre\-installed on the DLAMI\. If you want to run the latest, untested nightly build, you can [Install TensorFlow's Nightly Build \(experimental\)](#tutorial-tensorflow-install) manually\. 

**To run TensorFlow on the DLAMI with Conda**

1. To activate TensorFlow, open an Amazon Elastic Compute Cloud \(Amazon EC2\) instance of the DLAMI with Conda\.
   + For TensorFlow and Keras 2 on Python 3 with CUDA 9\.0 and MKL\-DNN, run this command:

     ```
     $ source activate tensorflow_p36
     ```
   + For TensorFlow and Keras 2 on Python 2 with CUDA 9\.0 and MKL\-DNN, run this command:

     ```
     $ source activate tensorflow_p27
     ```

1. Start the iPython terminal:

   ```
   (tensorflow_p36)$ ipython
   ```

1. Run a TensorFlow program to verify that it is working properly:

   ```
   import tensorflow as tf
   hello = tf.constant('Hello, TensorFlow!')
   sess = tf.Session()
   print(sess.run(hello))
   ```

   `Hello, TensorFlow!` should appear on your screen\.

## Install TensorFlow's Nightly Build \(experimental\)<a name="tutorial-tensorflow-install"></a>

You can install the latest TensorFlow build into either or both of the TensorFlow Conda environments on your Deep Learning AMI with Conda\.

**To install TensorFlow from a nightly build**

1. 
   + For the Python 3 TensorFlow environment, run the following command:

     ```
     $ source activate tensorflow_p36
     ```
   + For the Python 2 TensorFlow environment, run the following command:

     ```
     $ source activate tensorflow_p27
     ```

1. Remove the currently installed TensorFlow\.
**Note**  
The remaining steps assume you are using the `tensorflow_p36` environment\.

   ```
   (tensorflow_p36)$ pip uninstall tensorflow
   ```

1. Install the latest nightly build of TensorFlow\.

   ```
   (tensorflow_p36)$ pip install tf-nightly
   ```

1. To verify you have successfully installed latest nightly build, start the IPython terminal and check the version of TensorFlow\.

   ```
   (tensorflow_p36)$ ipython
   ```

   ```
   import tensorflow
   print (tensorflow.__version__)
   ```

   The output should print something similar to `1.12.0-dev20181012`

## More Tutorials<a name="tutorial-tensorflow-more"></a>

[TensorFlow with Horovod](tutorial-horovod-tensorflow.md)

[TensorBoard](tutorial-tensorboard.md)

[TensorFlow Serving](tutorial-tfserving.md)

For tutorials, see the folder called `Deep Learning AMI with Conda tutorials` in the home directory of the DLAMI\. 

For more tutorials and examples, see the TensorFlow documentation for the [TensorFlow Python API](https://www.tensorflow.org/api_docs/python/) or see the [TensorFlow](https://www.tensorflow.org) website\.