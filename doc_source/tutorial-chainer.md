# Chainer<a name="tutorial-chainer"></a>

**Note**  
We no longer include Chainer Conda environments in the AWS Deep Learning AMI starting with the v28 release\. Previous releases of the AWS Deep Learning AMI that contain these environments will continue to be available\. However, we will only provide updates to these environments if there are security fixes published by the open source community for these frameworks\.

[Chainer](https://chainer.org/) is a flexible Python\-based framework for easily and intuitively writing complex neural network architectures\. Chainer makes it easy to use multi\-GPU instances for training\. Chainer also automatically logs results, graph loss and accuracy, and produces output for visualizing the neural network with a [computational graph](https://docs.chainer.org/en/stable/reference/graph.html)\. It is included with the Deep Learning AMI with Conda \(DLAMI with Conda\)\. 

The following topics show you how to train on multiple GPUs, a single GPU, and a CPU, create visualizations, and test your Chainer installation\.

**Topics**
+ [Training a Model with Chainer](#tutorial-chainer-model)
+ [Use Chainer to Train on Multiple GPUs](#tutorial-chainer-multi-gpu)
+ [Use Chainer to Train on a Single GPU](#tutorial-chainer-gpu)
+ [Use Chainer to Train with CPUs](#tutorial-chainer-cpu)
+ [Graphing Results](#tutorial-chainer-graphing)
+ [Testing Chainer](#tutorial-chainer-project)
+ [More Info](#more-info-chainer-project)

## Training a Model with Chainer<a name="tutorial-chainer-model"></a>

This tutorial shows you how to use example Chainer scripts to train a model with the MNIST dataset\. MNIST is a database of handwritten numbers that is commonly used to train image recognition models\. The tutorial also shows the difference in training speed between training on a CPU and one or more GPUs\. 

## Use Chainer to Train on Multiple GPUs<a name="tutorial-chainer-multi-gpu"></a>

**To train on multiple GPUs**

1. Connect to the instance running Deep Learning AMI with Conda\. Refer to the [Selecting the Instance Type for DLAMI](instance-select.md) or the [Amazon EC2 documentation](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstances.html) on how to select or connect to an instance\. To run this tutorial, you will want to use an instance with at least two GPUs\.

1. Activate the Python 3 Chainer environment:

   ```
   $ source activate chainer_p36
   ```

1. To get the latest tutorials, clone the Chainer repository, and navigate to the examples folder:

   ```
   (chainer_p36) :~$ cd ~/src
   (chainer_p36) :~/src$ CHAINER_VERSION=v$(python -c "import chainer; print(chainer.__version__)")
   (chainer_p36) :~/src$ git clone -b $CHAINER_VERSION https://github.com/chainer/chainer.git
   (chainer_p36) :~/src$ cd chainer/examples/mnist
   ```

1. Run the example in the `train_mnist_data_parallel.py` script\. By default, the script uses the GPUs running on your instance of Deep Learning AMI with Conda\. The script can be run on a maximum of two GPUs\. It will ignore any GPUs past the first two\. It detects one or both automatically\. If you are running an instance without GPUs, skip to [Use Chainer to Train with CPUs](#tutorial-chainer-cpu), later in this tutorial\. 

   ```
   (chainer_p36) :~/src/chainer/examples/mnist$ python train_mnist_data_parallel.py
   ```
**Note**  
This example will return the following error due to the inclusion of a beta feature not included in the DLAMI\.  
`chainerx ModuleNotFoundError: No module named 'chainerx'`

    While the Chainer script trains a model using the MNIST database, you see the results for each epoch\.

   Then you see example output as the script runs\. The following example output was run on a p3\.8xlarge instance\. The script's output shows "GPU: 0, 1", which indicates that it is using the first two of the four available GPUs\. The scripts typically use an index of GPUs starting with zero, instead of a total count\.

   ```
   GPU: 0, 1
   
   # unit: 1000
   # Minibatch-size: 400
   # epoch: 20
   
   epoch       main/loss   validation/main/loss  main/accuracy  validation/main/accuracy  elapsed_time
   1           0.277561    0.114709              0.919933       0.9654                    6.59261       
   2           0.0882352   0.0799204             0.973334       0.9752                    8.25162       
   3           0.0520674   0.0697055             0.983967       0.9786                    9.91661       
   4           0.0326329   0.0638036             0.989834       0.9805                    11.5767       
   5           0.0272191   0.0671859             0.9917         0.9796                    13.2341       
   6           0.0151008   0.0663898             0.9953         0.9813                    14.9068       
   7           0.0137765   0.0664415             0.995434       0.982                     16.5649       
   8           0.0116909   0.0737597             0.996          0.9801                    18.2176       
   9           0.00773858  0.0795216             0.997367       0.979                     19.8797       
   10          0.00705076  0.0825639             0.997634       0.9785                    21.5388       
   11          0.00773019  0.0858256             0.9978         0.9787                    23.2003       
   12          0.0120371   0.0940225             0.996034       0.9776                    24.8587       
   13          0.00906567  0.0753452             0.997033       0.9824                    26.5167       
   14          0.00852253  0.082996              0.996967       0.9812                    28.1777       
   15          0.00670928  0.102362              0.997867       0.9774                    29.8308       
   16          0.00873565  0.0691577             0.996867       0.9832                    31.498        
   17          0.00717177  0.094268              0.997767       0.9802                    33.152        
   18          0.00585393  0.0778739             0.998267       0.9827                    34.8268       
   19          0.00764773  0.107757              0.9975         0.9773                    36.4819       
   20          0.00620508  0.0834309             0.998167       0.9834                    38.1389
   ```

1. While your training is running it is useful to look at your GPU utilization\. You can verify which GPUs are active and view their load\. NVIDIA provides a tool for this, which can be run with the command `nvidia-smi`\. However, it will only tell you a snapshot of the utilization, so it's more informative to combine this with the Linux command `watch`\. The following command will use `watch` with `nvidia-smi` to refresh the current GPU utilization every tenth of a second\. Open up another terminal session to your DLAMI, and run the following command: 

   ```
   (chainer_p36) :~$ watch -n0.1 nvidia-smi
   ```

   You will see an output similar to the following result\. Use `ctrl-c` to close the tool, or just keep it running while you try out other examples in your first terminal session\.

   ```
   Every 0.1s: nvidia-smi                                   Wed Feb 28 00:28:50 2018
   
   Wed Feb 28 00:28:50 2018
   +-----------------------------------------------------------------------------+
   | NVIDIA-SMI 384.111                Driver Version: 384.111                   |
   |-------------------------------+----------------------+----------------------+
   | GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
   | Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
   |===============================+======================+======================|
   |   0  Tesla V100-SXM2...  On   | 00000000:00:1B.0 Off |                    0 |
   | N/A   46C    P0    56W / 300W |    728MiB / 16152MiB |     10%      Default |
   +-------------------------------+----------------------+----------------------+
   |   1  Tesla V100-SXM2...  On   | 00000000:00:1C.0 Off |                    0 |
   | N/A   44C    P0    53W / 300W |    696MiB / 16152MiB |      4%      Default |
   +-------------------------------+----------------------+----------------------+
   |   2  Tesla V100-SXM2...  On   | 00000000:00:1D.0 Off |                    0 |
   | N/A   42C    P0    38W / 300W |     10MiB / 16152MiB |      0%      Default |
   +-------------------------------+----------------------+----------------------+
   |   3  Tesla V100-SXM2...  On   | 00000000:00:1E.0 Off |                    0 |
   | N/A   46C    P0    40W / 300W |     10MiB / 16152MiB |      0%      Default |
   +-------------------------------+----------------------+----------------------+
   
   +-----------------------------------------------------------------------------+
   | Processes:                                                       GPU Memory |
   |  GPU       PID   Type   Process name                             Usage      |
   |=============================================================================|
   |    0     54418      C   python                                       718MiB |
   |    1     54418      C   python                                       686MiB |
   +-----------------------------------------------------------------------------+
   ```

   In this example, GPU 0 and GPU 1 are active, and GPU 2 and 3 are not\. You can also see memory utilization per GPU\.

1. As training completes, note the elapsed time in your first terminal session\. In the example, elapsed time is 38\.1389 seconds\.

## Use Chainer to Train on a Single GPU<a name="tutorial-chainer-gpu"></a>

This example shows how to train on a single GPU\. You might do this if you have only one GPU available or just to see how multi\-GPU training might scale with Chainer\. 

**To use Chainer to train on a single GPU**
+ For this example, you use another script, `train_mnist.py`, and tell it to use just GPU 0 with the `--gpu=0` argument\. To see how a different GPUs activate in the `nvidia-smi` console, you can tell the script to use GPU number 1 by using `--gpu=1` \.

  ```
  (chainer_p36) :~/src/chainer/examples/mnist$ python train_mnist.py --gpu=0
  ```

  ```
  GPU: 0
  # unit: 1000
  # Minibatch-size: 100
  # epoch: 20
  
  epoch       main/loss   validation/main/loss  main/accuracy  validation/main/accuracy  elapsed_time
  1           0.192348    0.0909235             0.940934       0.9719                    5.3861        
  2           0.0746767   0.069854              0.976566       0.9785                    8.97146       
  3           0.0477152   0.0780836             0.984982       0.976                     12.5596       
  4           0.0347092   0.0701098             0.988498       0.9783                    16.1577       
  5           0.0263807   0.08851               0.991515       0.9793                    19.7939       
  6           0.0253418   0.0945821             0.991599       0.9761                    23.4643       
  7           0.0209954   0.0683193             0.993398       0.981                     27.0317       
  8           0.0179036   0.080285              0.994149       0.9819                    30.6325       
  9           0.0183184   0.0690474             0.994198       0.9823                    34.2469       
  10          0.0127616   0.0776328             0.996165       0.9814                    37.8693       
  11          0.0145421   0.0970157             0.995365       0.9801                    41.4629       
  12          0.0129053   0.0922671             0.995899       0.981                     45.0233       
  13          0.0135988   0.0717195             0.995749       0.9857                    48.6271       
  14          0.00898215  0.0840777             0.997216       0.9839                    52.2269       
  15          0.0103909   0.123506              0.996832       0.9771                    55.8667       
  16          0.012099    0.0826434             0.996616       0.9847                    59.5001       
  17          0.0066183   0.101969              0.997999       0.9826                    63.1294       
  18          0.00989864  0.0877713             0.997116       0.9829                    66.7449       
  19          0.0101816   0.0972672             0.996966       0.9822                    70.3686       
  20          0.00833862  0.0899327             0.997649       0.9835                    74.0063
  ```

  In this example, running on a single GPU took almost twice as long\! Training larger models or larger datasets will yield different results from this example, so experiment to further evaluate GPU performance\. 

## Use Chainer to Train with CPUs<a name="tutorial-chainer-cpu"></a>

Now try training on a CPU\-only mode\. Run the same script, `python train_mnist.py`, without arguments: 

```
(chainer_p36) :~/src/chainer/examples/mnist$ python train_mnist.py
```

In the output, `GPU: -1` indicates that no GPU is used:

```
GPU: -1
# unit: 1000
# Minibatch-size: 100
# epoch: 20

epoch       main/loss   validation/main/loss  main/accuracy  validation/main/accuracy  elapsed_time
1           0.192083    0.0918663             0.94195        0.9712                    11.2661       
2           0.0732366   0.0790055             0.977267       0.9747                    23.9823       
3           0.0485948   0.0723766             0.9844         0.9787                    37.5275       
4           0.0352731   0.0817955             0.987967       0.9772                    51.6394       
5           0.029566    0.0807774             0.990217       0.9764                    65.2657       
6           0.025517    0.0678703             0.9915         0.9814                    79.1276       
7           0.0194185   0.0716576             0.99355        0.9808                    93.8085       
8           0.0174553   0.0786768             0.994217       0.9809                    108.648       
9           0.0148924   0.0923396             0.994983       0.9791                    123.737       
10          0.018051    0.099924              0.99445        0.9791                    139.483       
11          0.014241    0.0860133             0.995783       0.9806                    156.132       
12          0.0124222   0.0829303             0.995967       0.9822                    173.173       
13          0.00846336  0.122346              0.997133       0.9769                    190.365       
14          0.011392    0.0982324             0.996383       0.9803                    207.746       
15          0.0113111   0.0985907             0.996533       0.9813                    225.764       
16          0.0114328   0.0905778             0.996483       0.9811                    244.258       
17          0.00900945  0.0907504             0.9974         0.9825                    263.379       
18          0.0130028   0.0917099             0.996217       0.9831                    282.887       
19          0.00950412  0.0850664             0.997133       0.9839                    303.113       
20          0.00808573  0.112367              0.998067       0.9778                    323.852
```

In this example, MNIST was trained in 323 seconds, which is more than 11x longer than training with two GPUs\. If you've ever doubted the power of GPUs, this example shows how much more efficient they are\.



## Graphing Results<a name="tutorial-chainer-graphing"></a>

Chainer also automatically logs results, graph loss and accuracy, and produces output for plotting the computational graph\. 

**To generate the computational graph**

1. After any training run finishes, you may navigate to the `result` directory and view the run's accuracy and loss in the form of two automatically generated images\. Navigate there now, and list the contents: 

   ```
   (chainer_p36) :~/src/chainer/examples/mnist$ cd result
   (chainer_p36) :~/src/chainer/examples/mnist/result$ ls
   ```

   The `result` directory contains two files in \.png format: `accuracy.png` and `loss.png`\. 

1. To view the graphs, use the `scp` command to copy them to your local computer\.

   In a macOS terminal, running the following `scp` command downloads all three files to your `Downloads` folder\. Replace the placeholders for the location of the key file and server address with your information\. For other operating systems, use the appropriate `scp` command format\. Note, for an Amazon Linux AMI, the user name is ec2\-user\.

   ```
   (chainer_p36) :~/src/chainer/examples/mnist/result$ scp -i "your-key-file.pem" ubuntu@your-dlami-address.compute-1.amazonaws.com:~/src/chainer/examples/mnist/result/*.png ~/Downloads
   ```

The following images are examples of accuracy, loss, and computational graphs, respectively\.

![\[MNIST training accuracy\]](http://docs.aws.amazon.com/dlami/latest/devguide/images/chainer-accuracy.png)

![\[MNIST training loss\]](http://docs.aws.amazon.com/dlami/latest/devguide/images/chainer-loss.png)

![\[MNIST training computational graph\]](http://docs.aws.amazon.com/dlami/latest/devguide/images/chainer-cg.png)

## Testing Chainer<a name="tutorial-chainer-project"></a>

To test Chainer and verify GPU support with a preinstalled test script, run the following command:

```
(chainer_p36) :~/src/chainer/examples/mnist/result$ cd ~/src/bin
(chainer_p36) :~/src/bin$ ./testChainer
```

This downloads Chainer source code and runs the Chainer multi\-GPU MNIST example\.

## More Info<a name="more-info-chainer-project"></a>

To learn more about Chainer, see the [Chainer documentation website](https://docs.chainer.org/)\. The `Chainer` examples folder contains more examples\. Try them to see how they perform\.