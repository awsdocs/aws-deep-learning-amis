# CNTK<a name="tutorial-cntk"></a>

## CNTK Tutorial<a name="tutorial-cntk-overview"></a>

To activate the framework, follow these instructions on your Deep Learning AMI with Conda\.

For Python 3 with CUDA 9 with cuDNN 7:

```
$ source activate cntk_p36
```

For Python 2 with CUDA 9 with cuDNN 7:

```
$ source activate cntk_p27
```

Start the iPython terminal\.

```
(caffe2_p27)$ ipython
```

Run a quick CNTK program\.

```
import cntk as C
C.__version__
c = C.constant(3, shape=(2,3))
c.asarray()
```

You should see the CNTK version, then the output of a 2x3 array of 3's\.

If you have a GPU instance, you can test it with the following code example\. A result of `True` is what you would expect if CNTK can access the GPU\.

```
from cntk.device import try_set_default_device, gpu
try_set_default_device(gpu(0))
```

## More Tutorials<a name="tutorial-cntk-more"></a>

For more tutorials and examples refer to the framework's official Python docs, [Python API for CNTK](https://cntk.ai/pythondocs/gettingstarted.html), and the [CNTK](https://www.microsoft.com/en-us/cognitive-toolkit/) website\.