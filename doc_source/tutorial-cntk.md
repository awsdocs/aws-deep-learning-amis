# CNTK<a name="tutorial-cntk"></a>

## Activating CNTK<a name="tutorial-cntk-overview"></a>

This tutorial shows how to activate CNTK on an instance running the Deep Learning AMI with Conda \(DLAMI on Conda\) and run a CNTK program\.

When a stable Conda package of a framework is released, it's tested and pre\-installed on the DLAMI\. If you want to run the latest, untested nightly build, you can [Install CNTK's Nightly Build \(experimental\)](#tutorial-cntk-install) manually\. 

**To run CNTK on the DLAMI with Conda**

1. To activate CNTK, open an Amazon Elastic Compute Cloud \(Amazon EC2\) instance of the DLAMI with Conda\.
   + For Python 3 with CUDA 9 with cuDNN 7:

     ```
     $ source activate cntk_p36
     ```
   + For Python 2 with CUDA 9 with cuDNN 7:

     ```
     $ source activate cntk_p27
     ```

1. Start the iPython terminal\.

   ```
   (cntk_p36)$ ipython
   ```

1. 
   + If you have a CPU instance, run this quick CNTK program\.

     ```
     import cntk as C
     C.__version__
     c = C.constant(3, shape=(2,3))
     c.asarray()
     ```

     You should see the CNTK version, then the output of a 2x3 array of 3's\.
   + If you have a GPU instance, you can test it with the following code example\. A result of `True` is what you would expect if CNTK can access the GPU\.

     ```
     from cntk.device import try_set_default_device, gpu
     try_set_default_device(gpu(0))
     ```

## Install CNTK's Nightly Build \(experimental\)<a name="tutorial-cntk-install"></a>

You can install the latest CNTK build into either or both of the CNTK Conda environments on your Deep Learning AMI with Conda\.

**To install CNTK from a nightly build**

1. 
   + For CNTK and Keras 2 on Python 3 with CUDA 9\.0 and MKL\-DNN, run this command:

     ```
     $ source activate cntk_p36
     ```
   + For CNTK and Keras 2 on Python 2 with CUDA 9\.0 and MKL\-DNN, run this command:

     ```
     $ source activate cntk_p27
     ```

1. The remaining steps assume you are using the `cntk_p36` environment\. Remove the currently installed CNTK\.

   ```
   (cntk_p36)$ pip uninstall cntk
   ```

1. To install the CNTK nightly build, you first need to find the version you want to install from the [CNTK nightly website\.](https://cntk.ai/nightly-linux.html)

1. 
   + \(Option for GPU instances\) \- To install the nightly build that was available at the time of this writing, you would use the following:

     ```
     (cntk_p36)$ pip install https://cntk.ai/PythonWheel/GPU/cntk_gpu-2.6rc0.dev20181015-cp36-cp36m-linux_x86_64.whl
     ```

     Replace the URL in the previous command with the GPU version for your current Python environment\.
   + \(Option for CPU instances\) \- To install the nightly build that was available at the time of this writing, you would use the following:

     ```
     (cntk_p36)$ pip install https://cntk.ai/PythonWheel/CPU-Only/cntk-2.6rc0.dev20181015-cp36-cp36m-linux_x86_64.whl
     ```

     Replace the URL in the previous command with the CPU version for your current Python environment\.

1. To verify you have successfully installed latest nightly build, start the IPython terminal and check the version of CNTK\.

   ```
   (cntk_p36)$ ipython
   ```

   ```
   import cntk
   print (cntk.__version__)
   ```

   The output should print something similar to `2.6-rc0.dev20181015`

## More Tutorials<a name="tutorial-cntk-more"></a>

For more tutorials and examples, see the framework's official Python docs, [Python API for CNTK](https://cntk.ai/pythondocs/gettingstarted.html), or the [CNTK](https://www.microsoft.com/en-us/cognitive-toolkit/) website\.