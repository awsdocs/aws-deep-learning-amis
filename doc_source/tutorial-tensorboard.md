# TensorBoard<a name="tutorial-tensorboard"></a>

 [TensorBoard](https://www.tensorflow.org/get_started/summaries_and_tensorboard) lets you to visually inspect and interpret your TensorFlow runs and graphs\. It runs a web server that serves a webpage for viewing and interacting with the TensorBoard visualizations\. 

TensorFlow and TensorBoard are preinstalled with the Deep Learning AMI with Conda \(DLAMI with Conda\)\. The DLAMI with Conda also includes an example script that uses TensorFlow to train an MNIST model with extra logging features enabled\. MNIST is a database of handwritten numbers that is commonly used to train image recognition models\. In this tutorial, you use the script to train an MNIST model, and TensorBoard and the logs to create visualizations\. 

**Topics**
+ [Train an MNIST Model and Visualize the Training with TensorBoard](#tutorial-tensorboard-example)
+ [More Info](#tutorial-tensorboard-project)

## Train an MNIST Model and Visualize the Training with TensorBoard<a name="tutorial-tensorboard-example"></a>

**Visualize MNIST model training with TensorBoard**

1. Connect to your Amazon Elastic Compute Cloud \(Amazon EC2\) instance of the DLAMI with Conda\. 

1. Activate the Python 2\.7 TensorFlow environment and navigate to the directory that contains the folder with the TensorBoard example scripts:

   ```
   $ source activate tensorflow_p27
   $ cd ~/examples/tensorboard/
   ```

1. Run the script that trains an MNIST model with extended logging enabled:

   ```
   $ python mnist_with_summaries.py
   ```

   The script writes the logs to `/tmp/tensorflow/mnist`\. 

1. Pass the location of the logs to `tensorboard`: 

   ```
   $ tensorboard --logdir=/tmp/tensorflow/mnist
   ```

   TensorBoard launches the visualization web server on port 6006\.

1. For easy access from your local browser, you can change the web server port to port 80 or another port\. Whichever port you use, you will need to open this port in the EC2 security group for your DLAMI\. You can also use port forwarding\. For instructions on changing your security group settings and port forwarding, see [Set up a Jupyter Notebook Server](setup-jupyter.md)\. The default settings are described in the next step\.
**Note**  
If you need to run both Jupyter server and a TensorBoard server, use a different port for each\. 

1. Open port 6006 \(or the port you assigned to the visualization web server\) on your EC2 instance\.

   1. Open your EC2 instance in the Amazon EC2console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

   1. In the Amazon EC2 console, choose **Network & Security**, then choose**Security Groups**\.

   1. For **Security Group**, , choose the one that was created most recently \(see the time stamp in the description\)\.

   1.  Choose the **Inbound** tab, and choose **Edit**\.

   1. Choose **Add Rule**\. 

   1. In the new row, type the followings: 

      **Type** : Custom **TCP Rule**

      **Protocol**: **TCP**

      **Port Range**: **6006** \(or the port that you assigned to the visualization server\)

      **Source**: **Custom IP \(specify address/range\)** 

1. Open the web page for the TensorBoard visualizations by using the public IP or DNS address of the EC2 instance that's running the DLAMI with Conda and the port that you opened for TensorBoard: 

   **http://** ***YourInstancePublicDNS*:6006**

## More Info<a name="tutorial-tensorboard-project"></a>

To learn more about TensorBoard, see the [TensorBoard website](https://www.tensorflow.org/get_started/summaries_and_tensorboard)\.