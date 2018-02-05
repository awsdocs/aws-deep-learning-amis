# TensorFlow Serving<a name="tutorial-tfserving"></a>

[TensorFlow Serving](https://www.tensorflow.org/serving/) is a flexible, high\-performance serving system for machine learning models\.

## Train and Serve an MNIST Model with TensorFlow Serving<a name="tutorial-tfserving-model"></a>

The `tensorflow-serving-api` is pre\-installed with Deep Learning AMI with Conda\! You will find an example scripts to train, export, and serve an MNIST model in `~/tutorials/TensorFlow/serving`\. For this tutorial we will export a model then serve it with the `tensorflow_model_server` application\. Finally, you can test the model server with an example client script\.

First, connect to your Deep Learning AMI with Conda and activate the Python 2\.7 TensorFlow environmenent\. The example scripts are not compatible with Python 3\.x\.

```
$ source activate tensorflow_p27
```

Now change directories to the serving example scripts folder\.

```
$ cd ~/tutorials/TensorFlow/serving
```

Run the script that will train and export an MNIST model\. As the script's only argument, you need to provide a folder location for it to save the model\. For now we can just put it in `mnist_model`\. The script will create the folder for you\.

```
$ python mnist_saved_model.py /tmp/mnist_model
```

 Be patient, as this script may take a while before providing any output\. When the training is complete and the model is finally exported you should see the following: 

```
          Done training!
          Exporting trained model to mnist_model/1
          Done exporting!
```

Your next step is to run `tensorflow_model_server` to serve the exported model\. 

```
$ tensorflow_model_server --port=9000 --model_name=mnist --model_base_path=/tmp/mnist_model
```

A client script is provided for you to test the server\.

**To test it out, you will need to open a new terminal window\.**

```
$ python mnist_client.py --num_tests=1000 --server=localhost:9000
```

## More Features and Examples<a name="tutorial-tfserving-project"></a>

If you are interested in learning more about TensorFlow Serving, check out the [TensorFlow website](https://www.tensorflow.org/serving/)\.