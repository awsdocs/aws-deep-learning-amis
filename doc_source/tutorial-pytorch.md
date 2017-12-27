# PyTorch<a name="tutorial-pytorch"></a>

To activate the framework, follow these instructions on your Deep Learning AMI with Conda\.

For Python 3 with CUDA 8:

```
$ source activate pytorch_p36
```

For Python 2 with CUDA 8:

```
$ source activate pytorch_p27
```

Start the iPython terminal\.

```
(pytorch_p36)$ ipython
```

Run a quick PyTorch program\.

```
import torch
x = torch.rand(5, 3)
print(x)
print(x.size())
y = torch.rand(5, 3)
print(torch.add(x, y))
```

You should see the initial random array printed, then its size, and then the addition of another random array\.

**More Tutorials**  
You can find more tutorials in the Deep Learning AMI with Conda tutorials folder in the home directory of the DLAMI\. For further tutorials and examples refer to the framework's official docs, [PyTorch documentation](http://pytorch.org/docs/master/), and the [PyTorch](http://pytorch.org) website\.