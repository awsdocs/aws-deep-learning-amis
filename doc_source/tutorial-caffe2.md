# Caffe2<a name="tutorial-caffe2"></a>

To activate the framework, follow these instructions on your Deep Learning AMI with Conda\.

There is only the Python 2 with CUDA 9 option:

```
$ source activate caffe2_p27
```

Start the iPython terminal\.

```
(caffe2_p27)$ ipython
```

Run a quick Caffe2 program\.

```
from caffe2.python import workspace, model_helper
import numpy as np
# Create random tensor of three dimensions
x = np.random.rand(4, 3, 2)
print(x)
print(x.shape)
workspace.FeedBlob("my_x", x)
x2 = workspace.FetchBlob("my_x")
print(x2)
```

You should see the initial numpy random arrays printed and then those loaded into a Caffe2 blob\. Note that after loading they are the same\.

**More Tutorials**  
For more tutorials and examples refer to the framework's official Python docs, [Python API for Caffe2](https://caffe2.ai/doxygen-python/html/annotated.html), and the [Caffe2](https://caffe2.ai) website\.