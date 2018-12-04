# TensorFlow with Horovod<a name="activate-horovod"></a>

This tutorial shows how to activate TensorFlow with Horovod on an AWS Deep Learning AMI \(DLAMI\) with Conda\. Horovod is pre\-installed in the Conda environments for TensorFlow\. The Python3 environment is recommended\. 

**Note**  
Only P3\.\*, P2\.\*, and G3\.\* instance types are supported\.

**To activate TensorFlow and test Horovod on the DLAMI with Conda**

1. Open an Amazon Elastic Compute Cloud \(Amazon EC2\) instance of the DLAMI with Conda\. For help getting started with a DLAMI, see [How to Get Started with the DLAMI](gs.md#getting-started)\.
   + \(Recommended\) For TensorFlow with Horovod on Python 3 with CUDA 9, run this command:

     ```
     $ source activate tensorflow_p36
     ```
   + For TensorFlow with Horovod on Python 2 with CUDA 9, run this command:

     ```
     $ source activate tensorflow_p27
     ```

1. Start the iPython terminal:

   ```
   (tensorflow_p36)$ ipython
   ```

1. Test importing TensorFlow with Horovod to verify that it's working properly:

   ```
   import horovod.tensorflow as hvd
   hvd.init()
   ```

   The following should appear on your screen \(you may ignore any warning messages\)\.

   ```
   --------------------------------------------------------------------------
   [[55425,1],0]: A high-performance Open MPI point-to-point messaging module
   was unable to find any relevant network interfaces:
   
   Module: OpenFabrics (openib)
     Host: ip-172-31-72-4
   
   Another transport will be used instead, although this may result in
   lower performance.
   --------------------------------------------------------------------------
   ```

## More Info<a name="activate-horovod-project"></a>
+ [TensorFlow with Horovod](tutorial-horovod-tensorflow.md)
+ For tutorials, see the `examples/horovod` folder in the home directory of the DLAMI\. 
+ For even more tutorials and examples, see the [Horovod GitHub project](https://github.com/uber/horovod)\.