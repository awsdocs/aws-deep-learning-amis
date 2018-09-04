# Keras with MXNet<a name="keras-mxnet"></a>

This tutorial shows how to activate and use Keras 2 with the MXNet backend on a Deep Learning AMI with Conda\.

**Activate Keras with the MXNet backend and test it on the DLAMI with Conda**

1. To activate Keras with the MXNet backend, open an Amazon Elastic Compute Cloud \(Amazon EC2\) instance of the DLAMI with Conda\.
   + For Python 3, run this command:

     ```
     $ source activate mxnet_p36
     ```
   + For Python 2, run this command:

     ```
     $ source activate mxnet_p27
     ```

1. Start the iPython terminal:

   ```
   (mxnet_p36)$ ipython
   ```

1. Test importing Keras with MXNet to verify that it is working properly:

   ```
   import keras as k
   ```

   The following should appear on your screen \(possibly after a few warning messages\)\.

   ```
   Using MXNet backend
   ```
**Note**  
If you get an error, or if the TensorFlow backend is still being used, you need to update your Keras config manually\. Edit the `~/.keras/keras.json` file and change the backend setting to `mxnet`\.

## Keras\-MXNet Multi\-GPU Training Tutorial<a name="tutorial-keras-mxnet"></a>

**Train a convolutional neural network \(CNN\)**

1. Open a terminal and SSH into your DLAMI\.

1. Navigate to the `~/examples/keras-mxnet/` folder\.

1. Run `nvidia-smi` in your terminal window to determine the number of available GPUs on your DLAMI\. In the next step, you will run the script as\-is if you have four GPUs\. 

1. \(Optional\) Run the following command to open the script for editing\.

   ```
   (mxnet_p36)$ vi cifar10_resnet_multi_gpu.py
   ```

1. \(Optional\) The script has the following line that defines the number of GPUs\. Update it if necessary\.

   ```
   model = multi_gpu_model(model, gpus=4)
   ```

1. Now, run the training\.

   ```
   (mxnet_p36)$ python cifar10_resnet_multi_gpu.py
   ```

**Note**  
Keras\-MXNet runs up to two times faster with the `channels_first` image\_data\_format set\. To change to `channels_first`, edit your Keras config file \(`~/.keras/keras.json`\) and set the following: `"image_data_format": "channels_first"`\.  
For more performance tuning techniques, see [Keras\-MXNet performance tuning guide](https://github.com/awslabs/keras-apache-mxnet/blob/master/docs/mxnet_backend/performance_guide.md)\. 

## More Info<a name="tutorial-keras-mxnet-project"></a>
+ You can find examples for Keras with a MXNet backend in the Deep Learning AMI with Conda `~/examples/keras-mxnet` directory\.
+ For even more tutorials and examples, see the [Keras\-MXNet GitHub project](https://github.com/awslabs/keras-apache-mxnet)\.