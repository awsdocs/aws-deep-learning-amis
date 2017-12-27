# TensorFlow<a name="tutorial-tensorflow"></a>

To activate the framework, follow these instructions on your Deep Learning AMI with Conda\.

For TensorFlow \+ Keras 2 on Python 3 with CUDA 8:

```
$ source activate tensorflow_p36
```

For TensorFlow \+ Keras 2 on Python 2 with CUDA 8:

```
$ source activate tensorflow_p27
```

Start the iPython terminal\.

```
(tensorflow_p36)$ ipython
```

Run a quick TensorFlow program\.

```
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```

You should see "Hello, Tensorflow\!"

**More Tutorials**  
You can find more tutorials in the Deep Learning AMI with Conda tutorials folder in the home directory of the DLAMI\. For further tutorials and examples refer to the framework's official docs, [TensorFlow Python API](https://www.tensorflow.org/api_docs/python/), and the [TensorFlow](https://www.tensorflow.org) website\.