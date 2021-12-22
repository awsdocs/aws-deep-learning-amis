# Launching a Habana DLAMI<a name="tutorial-habana-launching"></a>

The latest DLAMI is ready to use with Habana Gaudi accelerators\. Use the following steps to launch your Habana DLAMI and ensure that your Python and framework\-specific resources are active\. For additional setup resources, see the [Habana Gaudi Setup and Installation](https://github.com/HabanaAI/Setup_and_Install) respository\. 

**Topics**
+ [Select a Habana DLAMI](#tutorial-habana-select-dlami)
+ [Activate Python Environment](#tutorial-habana-activate-python)
+ [Import Machine Learning Framework](#tutorial-habana-import-framework)

## Select a Habana DLAMI<a name="tutorial-habana-select-dlami"></a>

Launch a [DL1 instance](https://aws.amazon.com/ec2/instance-types/dl1/) with the Habana DLAMI of your choice\.

For step\-by\-step instructions on launching a DLAMI, see [Launching and Configuring a DLAMI\.](https://docs.aws.amazon.com/dlami/latest/devguide/launch-config.html)

For a list of the most recent Habana DLAMIs, see the [Release Notes for DLAMI](https://docs.aws.amazon.com/dlami/latest/devguide/appendix-ami-release-notes.html)\.

## Activate Python Environment<a name="tutorial-habana-activate-python"></a>

Connect to your DL1 instance and activate the recommended Python environment for your Habana DLAMI\. To check your recommended Python environment, select your DLAMI in the [Release Notes](https://docs.aws.amazon.com/dlami/latest/devguide/appendix-ami-release-notes.html)\.

## Import Machine Learning Framework<a name="tutorial-habana-import-framework"></a>

Instances with Habana accelerators are pre\-integrated with popular machine learning frameworks such as TensorFlow and PyTorch\. Import the machine learning framework of your choice\.

### Import TensorFlow<a name="tutorial-habana-import-tensorflow"></a>

To use TensorFlow on your Habana DLAMI, navigate to the folder of the Python environment that you activated and import TensorFlow\.

```
/usr/bin/$PYTHON_VERSION
import tensorflow
tensorflow.__version__
```

To check the TensorFlow version compatible with your Habana DLAMI, select your DLAMI in the [Release Notes](https://docs.aws.amazon.com/dlami/latest/devguide/appendix-ami-release-notes.html)\.

### Import PyTorch<a name="tutorial-habana-import-pytorch"></a>

To use PyTorch on your Habana DLAMI, navigate to the folder of the Python environment that you activated and import the appropriate PyTorch version\.

```
/usr/bin/$PYTHON_VERSION
import torch
torch.__version__
```

To check the PyTorch version compatible with your Habana DLAMI, select your DLAMI in the [Release Notes](https://docs.aws.amazon.com/dlami/latest/devguide/appendix-ami-release-notes.html)\.

For more information on how to run and train machine learning models in TensorFlow and PyTorch using your Habana DLAMI, see the [Habana Model References](https://github.com/HabanaAI/Model-References) GitHub repository\. For additional resources on working with your Habana DLAMI, visit the [Habana Gaudi documentation](https://docs.habana.ai/en/latest/)\.