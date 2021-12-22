# Chainer<a name="activate-chainer"></a>

**Note**  
We no longer include Chainer Conda environments in the AWS Deep Learning AMI starting with the v28 release\. Previous releases of the AWS Deep Learning AMI that contain these environments will continue to be available\. However, we will only provide updates to these environments if there are security fixes published by the open source community for these frameworks\.

[Chainer](https://chainer.org/) is a flexible Python\-based framework for easily and intuitively writing complex neural network architectures\. Chainer makes it easy to use multi\-GPU instances for training\. Chainer also automatically logs results, graph loss and accuracy, and produces output for visualizing the neural network with a [computational graph](https://docs.chainer.org/en/stable/reference/graph.html)\. It is included with the Deep Learning AMI with Conda \(DLAMI with Conda\)\. 

**Activate Chainer**

1. Connect to the instance running Deep Learning AMI with Conda\. Refer to the [Selecting the Instance Type for DLAMI](instance-select.md) or the [Amazon EC2 documentation](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) on how to select or connect to an instance\.

1. 
   + Activate the Python 3 Chainer environment:

     ```
     $ source activate chainer_p36
     ```
   + Activate the Python 2 Chainer environment:

     ```
     $ source activate chainer_p27
     ```

1. Start the iPython terminal:

   ```
   (chainer_p36)$ ipython
   ```

1. Test importing Chainer to verify that it is working properly:

   ```
   import chainer
   ```

   You may see a few warning messages, but no error\.

## More Info<a name="activate-chainer-project"></a>
+ Try the tutorials for [Chainer](tutorial-chainer.md)\.
+ The `Chainer` examples folder inside the source you downloaded earlier contains more examples\. Try them to see how they perform\.
+ To learn more about Chainer, see the [Chainer documentation website](https://docs.chainer.org/)\.