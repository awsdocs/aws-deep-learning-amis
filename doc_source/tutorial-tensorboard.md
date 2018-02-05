# TensorBoard<a name="tutorial-tensorboard"></a>

[TensorBoard](https://www.tensorflow.org/get_started/summaries_and_tensorboard) is used to visualize your TensorFlow graph, plot quantitative metrics about the execution of your graph, and show additional data like images that pass through it\.

## Train an MNIST Model then Visualize with TensorBoard<a name="tutorial-tensorboard-example"></a>

The `tensorboard` tool is pre\-installed with Deep Learning AMI with Conda\! You will find an example script to train MNIST model with extra logging features in `~/tutorials/TensorFlow/board`\. These logs are used by TensorBoard to create visualizations\. TensorBoard runs a webserver to serve a web page for viewing and interacting with the visualizations\.

First, connect to your Deep Learning AMI with Conda and activate the Python 2\.7 TensorFlow environmenent\. Then change directories to the TensorBoard example scripts folder\.

```
$ source activate tensorflow_p27
$ cd ~/tutorials/TensorFlow/board
```

Run the script that will train the MNIST model while taking care of extended logging\.

```
$ python mnist_with_summaries.py
```

 The script is hard\-coded to write the logs to `/tmp/tensorflow/mnist`\. This location is the parameter you need to pass to `tensorboard`: 

```
$ tensorboard --logdir=/tmp/tensorflow/mnist
```

By default it will launch the visualization server on port 6006\. Changing this to port 80 or another port may be required for ease of access from your local browser\. You will need to open this port in the EC2 Security Group for your DLAMI\. You can also use port forwarding\. Instructions for changing your security group settings and port forwarding are just like how you [Set up a Jupyter Notebook Server](setup-jupyter.md)\. Note that if you want to run both Jupyter server and a TensorBoard server, you need to pick different ports for each\. 

To open port 6006 \(or one of your choosing\) on your instance's firewall:

Open your EC2 dashboard and choose **Security Groups** on the EC2 navigation bar in the **Network & Security** section\. On this page, there will be one or more security groups in the list\. Find the most recent one \(the description has a timestamp in it\), select it, choose the **Inbound** tab, and then click **Edit**\. Then click **Add Rule**\. This adds a new row\. Fill in the fields with the following information: 

**Type** : Custom **TCP Rule**

**Protocol**: **TCP**

**Port Range**: **6006**

**Source**: **Anywhere** \(**0\.0\.0\.0/0,::/0**\)

Open the web page for the TensorBoard visualizations using the public IP or DNS of your DLAMI and the port you opened for TensorBoard: 

**http://** ***YourInstancePublicDNS*:6006**

## More Features and Examples<a name="tutorial-tensorboard-project"></a>

If you are interested in learning more about TensorBoard, visit the [TensorBoard website](https://www.tensorflow.org/get_started/summaries_and_tensorboard)\.