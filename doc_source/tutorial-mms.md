# MXNet Model Server<a name="tutorial-mms"></a>

[MXNet Model Server \(MMS\)](https://github.com/awslabs/mxnet-model-server/) is a flexible and easy to use tool for serving deep learning models exported from [MXNet](http://mxnet.io/)\.

## Serve a Deep Learning Model in Two Minutes<a name="tutorial-mms-serve-mxnet-model"></a>

MMS comes preinstalled with Deep Learning AMI with Conda\! You can get MMS model serving up and running very quickly with just a few commands\. 

 For this tutorial, we will skip over most of the features, but be sure to take a look at the [MMS documentation](https://github.com/awslabs/mxnet-model-server/tree/master/docs) when you're ready for more\. Here is an easy example for serving an image classification model\. You run the server, which will listen for prediction requests\. Then you upload an image, and the server will return a prediction of the top 5 out of 1,000 classes that is was trained on\.

First, connect to your Deep Learning AMI with Conda and activate an MXNet environmenent\.

```
$ source activate mxnet_p36
```

Then, run the `mxnet-model-server` with this one call that downloads a model for you and serves it\.

```
$ mxnet-model-server \
--models squeezenet=https://s3.amazonaws.com/model-server/models/squeezenet_v1.1/squeezenet_v1.1.model
```

With the command above executed, you have MMS running on your host, listening for inference requests\. 

**To test it out, you will need to open a new terminal window\.**

Then connect to the DLAMI, use use the commands below to download a kitten image, and send it to the MMS predict endpoint\. 

```
$ curl -O https://s3.amazonaws.com/model-server/inputs/kitten.jpg
$ curl -X POST http://127.0.0.1:8080/squeezenet/predict -F "data=@kitten.jpg"
```

 The predict endpoint will return a prediction response in JSON\. It will look something like the following result: 

```
        {
        "prediction": [
        [
        {
        "class": "n02124075 Egyptian cat",
        "probability": 0.940
        },
        {
        "class": "n02127052 lynx, catamount",
        "probability": 0.055
        },
        {
        "class": "n02123045 tabby, tabby cat",
        "probability": 0.002
        },
        {
        "class": "n02123159 tiger cat",
        "probability": 0.0003
        },
        {
        "class": "n02123394 Persian cat",
        "probability": 0.0002        }
        ]
        ]
        }
```

## Multi\-class Detection Example<a name="tutorial-mms-ssd-example"></a>

On your DLAMI you will find an example application using MMS with a Single Shot Detection \(SSD\) model\. From your DLAMI's terminal browse to the `~/tutorials/MXNet-Model-Server/ssd` folder\. The instructions on how to run the example are in the `README.md` file, or the instructions can be viewed on the MMS repository's [latest version of the example](https://github.com/awslabs/mxnet-model-server/blob/master/examples/ssd/README.md)\.

## More Features and Examples<a name="tutorial-mms-project"></a>

If you are interested in more examples, like how to export models, set up MMS with Docker, or to take advantage of the latest features, so be sure to star [MMS's project page](https://github.com/awslabs/mxnet-model-server)\. 