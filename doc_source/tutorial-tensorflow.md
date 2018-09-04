# TensorFlow<a name="tutorial-tensorflow"></a>

This tutorial shows how to activate TensorFlow on an instance running the Deep Learning AMI with Conda \(DLAMI on Conda\) and run a TensorFlow program\.

**To run TensorFlow on the DLAMI with Conda**

1. To activate TensorFlow, open an Amazon Elastic Compute Cloud \(Amazon EC2\) instance of the DLAMI with Conda\.
   + For TensorFlow and Keras 2 on Python 3 with CUDA 8, run this command:

     ```
     $ source activate tensorflow_p36
     ```
   + For TensorFlow and Keras 2 on Python 2 with CUDA 8, run this command:

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

## More Info<a name="tutorial-tensorflow-project"></a>

[TensorBoard](tutorial-tensorboard.md)

[TensorFlow Serving](tutorial-tfserving.md)

For tutorials, see the folder called `Deep Learning AMI with Conda tutorials`in the home directory of the DLAMI\. 

For even more tutorials and examples, see the TensorFlow documentation for the [TensorFlow Python API](https://www.tensorflow.org/api_docs/python/), and visit the [TensorFlow](https://www.tensorflow.org) website\.