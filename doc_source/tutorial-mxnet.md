# Apache MXNet<a name="tutorial-mxnet"></a>

To activate the framework, follow these instructions on your Deep Learning AMI with Conda\.

For Python 3 with CUDA 9:

```
$ source activate mxnet_p36
```

For Python 2 with CUDA 9:

```
$ source activate mxnet_p27
```

Start the iPython terminal\.

```
(mxnet_p36)$ ipython
```

Run a quick MXNet program\. Create a 5x5 matrix, an instance of the `NDArray`, with elements initialized to 0\. Print the array\.

```
import mxnet as mx
mx.ndarray.zeros((5,5)).asnumpy()
```

Verify the result\.

```
array([[ 0.,  0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.,  0.]], dtype=float32)
```

**More Tutorials**  
You can find more tutorials in the Deep Learning AMI with Conda tutorials folder in the home directory of the DLAMI\. For further tutorials and examples refer to the framework's official Python docs, [Python API for MXNet](https://mxnet.incubator.apache.org/api/python/index.html), and the [Apache MXNet](https://mxnet.incubator.apache.org/) website\.