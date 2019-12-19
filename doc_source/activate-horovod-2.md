# TensorFlow 2 with Horovod<a name="activate-horovod-2"></a>

This tutorial shows how to activate TensorFlow 2 with Horovod on an AWS Deep Learning AMI \(DLAMI\) with Conda\. Horovod is pre\-installed in the Conda environments for TensorFlow 2\. The Python3 environment is recommended\. 

**Note**  
Only P3\.\*, P2\.\*, and G3\.\* instance types are supported\.

**To activate TensorFlow 2 and test Horovod on the DLAMI with Conda**

1. Open an Amazon Elastic Compute Cloud \(Amazon EC2\) instance of the DLAMI with Conda\. For help getting started with a DLAMI, see [How to Get Started with the DLAMI](gs.md#getting-started)\.
   + \(Recommended\) For TensorFlow 2 with Horovod on Python 3 with CUDA 10, run this command:

     ```
     $ source activate tensorflow2_p36
     ```
   + For TensorFlow 2 with Horovod on Python 2 with CUDA 10, run this command:

     ```
     $ source activate tensorflow2_p27
     ```

1. Start the iPython terminal:

   ```
   (tensorflow2_p36)$ ipython
   ```

1. Test importing TensorFlow 2 with Horovod to verify that it's working properly:

   ```
   import horovod.tensorflow as hvd
   hvd.init()
   ```

   If you don't receive any output, then Horovod is working properly\. The following may appear on your screen \(you may ignore any warning messages\)\.

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

## More Info<a name="activate-horovod-2-project"></a>
+ For tutorials, see the `examples/horovod` folder in the home directory of the DLAMI\. 
+ For even more tutorials and examples, see the [Horovod GitHub project](https://github.com/uber/horovod)\.