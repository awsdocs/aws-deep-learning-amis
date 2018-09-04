# Keras<a name="tutorial-keras"></a>

## Keras Tutorial<a name="tutorial-keras-overview"></a>

1. To activate the framework, use these commands on your [Using the Deep Learning AMI with Conda](tutorial-conda.md) CLI\.
   + For Keras 2 with an MXNet backend on Python 3 with CUDA 9 with cuDNN 7:

     ```
     $ source activate mxnet_p36
     ```
   + For Keras 2 with an MXNet backend on Python 2 with CUDA 9 with cuDNN 7:

     ```
     $ source activate mxnet_p27
     ```
   + For Keras 2 with a TensorFlow backend on Python 3 with CUDA 9 with cuDNN 7:

     ```
     $ source activate tensorflow_p36
     ```
   + For Keras 2 with a TensorFlow backend on Python 2 with CUDA 9 with cuDNN 7:

     ```
     $ source activate tensorflow_p27
     ```

1. To test importing Keras to verify which backend is activated, use these commands:

   ```
   $ ipython
   import keras as k
   ```

   The following should appear on your screen:

   ```
   Using MXNet backend
   ```

   If Keras is using TensorFlow, the following is displayed:

   ```
   Using TensorFlow backend
   ```
**Note**  
If you get an error, or if the wrong backend is still being used, you can update your Keras configuration manually\. Edit the `~/.keras/keras.json` file and change the backend setting to `mxnet` or `tensorflow`\.

## More Tutorials<a name="tutorial-keras-more"></a>
+ For a multi\-GPU tutorial using Keras with a MXNet backend, try the [Keras\-MXNet Multi\-GPU Training Tutorial](keras-mxnet.md#tutorial-keras-mxnet)\.
+ You can find examples for Keras with a MXNet backend in the Deep Learning AMI with Conda `~/examples/keras-mxnet` directory\.
+ You can find examples for Keras with a TensorFlow backend in the Deep Learning AMI with Conda `~/examples/keras` directory\.
+ For additional tutorials and examples, see the [Keras](https://keras.io/) website\.