# TensorFlow with Horovod<a name="tutorial-horovod-tensorflow"></a>

This tutorial shows how to use TensorFlow with Horovod on a Deep Learning AMI with Conda\. Horovod is preinstalled in the Conda environments for TensorFlow\. The Python 3 environment is recommended\. The instructions here assume you have a working DLAMI instance with one or more GPUs\. For more information, see [How to Get Started with the DLAMI](gs.md#getting-started)\.

**Note**  
Only P3\.\*, P2\.\*, and G3\.\* instance types are supported\.

**Note**  
There are two locations where mpirun \(via OpenMPI\) is available\. It is available in `/usr/bin` and `/home/ubuntu/anaconda3/envs/<env>/bin`\. `env` is an environment corresponding to the framework, such as Tensorflow and Apache MXNet\. The mpirun binary in `/usr/bin` outside of the conda environment exists to maintain compatibility with the CNTK framework\. This framework expects v1\.10 at that location\. The newer OpenMPI versions are available in the conda environments\. We recommend using the absolute path of the mpirun binary or the [\-\-prefix flag](https://www.open-mpi.org/faq/?category=running#mpirun-prefix) to run mpi workloads\. For example, with the Tensorflow python36 environment, use either:   

```
/home/ubuntu/anaconda3/envs/tensorflow_p36/bin/mpirun <args>

or

mpirun --prefix /home/ubuntu/anaconda3/envs/tensorflow_p36/bin <args>
```

## Activate and Test TensorFlow with Horovod<a name="tutorial-horovod-tensorflow-activate"></a>

1. Verify that your instance has active GPUs\. NVIDIA provides a tool for this:

   ```
   $ nvidia-smi
   ```

1. Activate the Python 3 TensorFlow environment:

   ```
   $ source activate tensorflow_p36
   ```

1. Start the iPython terminal:

   ```
   (tensorflow_p36)$ ipython
   ```

1. Test importing TensorFlow with Horovod to verify that it is working properly:

   ```
   import horovod.tensorflow as hvd
   hvd.init()
   ```

   The following may appear on your screen \(possibly after a few warning messages\)\.

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

## Configure Your Horovod Hosts File<a name="tutorial-horovod-tensorflow-configure"></a>

You can use Horovod for single\-node, multi\-GPU training, or for multiple\-node, multi\-GPU training\. If you plan to use multiple nodes for distributed training, you must add each DLAMI private IP address to a hosts file\. The DLAMI you are currently logged into is referred to as the leader\. Other DLAMI instances that are part of the cluster are referred to as members\.

Before you start this section, launch one or more DLAMI, and wait for them to be in the **Ready** state\. The example scripts expect a hosts file, so even if you plan to use only one DLAMI, create a hosts file with only one entry\. If you edit the hosts file after training commences, you must restart training for added or removed hosts to take effect\.

**To configure Horovod for training**

1. Change directories to where the training scripts reside\.

   ```
   cd ~/examples/horovod/tensorflow
   ```

1.  Use vim to edit a file in the leader's home directory\. 

   ```
   vim hosts
   ```

1.  Select one of the members in the Amazon Elastic Compute Cloud console, and the description pane of the console appears\. Find the **Private IPs** field and copy the IP and paste it in a text file\. Copy each member's private IP on a new line\. Then, next to each IP, add a space and then the text `slots=8` as shown below\. This represents how many GPUs each instance has\. The p3\.16xlarge instances have 8 GPUs, so if you chose a different instance type, you would provide the actual number of GPUs for each instance\. For the leader you can use `localhost`\. With a cluster of 4 nodes, it should look similar to the following:

   ```
   172.100.1.200 slots=8
   172.200.8.99 slots=8
   172.48.3.124 slots=8
   localhost slots=8
   ```

    Save the file and exit back to the leader's terminal\.

1. Now your leader knows how to reach each member\. This is all going to happen on the private network interfaces\. Next, use a short bash function to help send commands to each member\.

   ```
   function runclust(){ while read -u 10 host; do host=${host%% slots*}; ssh -o "StrictHostKeyChecking no" $host ""$2""; done 10<$1; };
   ```

1. First order of the day is to tell the other members to not do “StrickHostKeyChecking” as this may cause training to hang\.

   ```
   runclust hosts "echo \"StrictHostKeyChecking no\" >> ~/.ssh/config"
   ```

## Train with Synthetic Data<a name="tutorial-horovod-tensorflow-synthetic"></a>

Your DLAMI ships with an example script to train a model with synthetic data\. This tests whether your leader can communicate with the members of the cluster\. A hosts file is required\. Refer to [Configure Your Horovod Hosts File](#tutorial-horovod-tensorflow-configure) for instructions\.

**To test Horovod training with example data**

1. `~/examples/horovod/tensorflow/train_synthetic.sh` defaults to 8 GPUs, but you can provide it the number of GPUs you want to run\. The following example runs the script, passing 4 as a parameter for 4 GPUs\.

   ```
   $ ./train_synthetic.sh 4
   ```

    After some warning messages, you see the following output that verifies Horovod is using 4 GPUs\.

   ```
   PY3.6.5 |Anaconda custom (64-bit)| (default, Apr 29 2018, 16:14:56) [GCC 7.2.0]TF1.11.0Horovod size: 4
   ```

    Then, after some other warnings, you see the start of a table and some data points\. If you don't want to watch for 1,000 batches, break out of the training\.

   ```
       Step Epoch  Speed  Loss   FinLoss LR
       0   0.0   105.6  6.794  7.708 6.40000
       1   0.0   311.7  0.000  4.315 6.38721
       100   0.1  3010.2  0.000 34.446 5.18400
       200   0.2  3013.6  0.000 13.077 4.09600
       300   0.2  3012.8  0.000  6.196 3.13600
       400   0.3  3012.5  0.000  3.551 2.30401
   ```

1. Horovod uses all local GPUs first before attempting to use the GPUs of the members of the cluster\. So, to make sure distributed training across the cluster is working, try out the full number of GPUs you intend to use\. If, for example, you have 4 members that are p3\.16xlarge instance type, you have 32 GPUs across your cluster\. This is where you would want to try out the full 32GPUs\.

   ```
   ./train_synthetic.sh 32
   ```

    Your output is similar to the previous test\. The Horovod size is 32, and roughly four\-times the speed\. With this experimentation completed, you have tested your leader and its ability to communicate with the members\. If you run into any issues, check the [Troubleshooting](#tutorial-horovod-troubleshooting) section\.

## Prepare the ImageNet Dataset<a name="tutorial-horovod-tensorflow-imagenet-prep"></a>

In this section, you download the ImageNet dataset, then generate a TFRecord\-format dataset from the raw dataset\. A set of preprocessing scripts is provided on the DLAMI for the ImageNet dataset that you can use for either ImageNet or as a template for another dataset\. The main training scripts that are configured for ImageNet are also provided\. The following section assumes that you have launched a DLAMI with an EC2 instance with 8 GPUs\. We recommend the p3\.16xlarge instance type\. 

In the `~/examples/horovod/tensorflow/utils` directory on your DLAMI you find the following scripts: 
+ `utils/preprocess_imagenet.py` \- Use this to convert the raw ImageNet dataset to the `TFRecord` format\.
+ `utils/tensorflow_image_resizer.py` \- Use this to resize the `TFRecord` dataset as recommended for ImageNet training\.

**Prepare the ImageNet Dataset**

1. Visit [image\-net\.org](http://image-net.org), create an account, acquire an access key, and download the dataset\. [image\-net\.org](http://image-net.org) hosts the raw dataset\. To download it, you are required to have an ImageNet account and an access key\. The account is free, and to get the free access key you must agree to the ImageNet license\. 

1. Use the image preprocessing script to generate a TFRecord format dataset from the raw ImageNet dataset\. From the `~/examples/horovod/tensorflow/utils` directory:

   ```
   python preprocess_imagenet.py \
            --local_scratch_dir=[YOUR DIRECTORY] \
            --imagenet_username=[imagenet account] \
            --imagenet_access_key=[imagenet access key]
   ```

1. Use the image resizing script\. If you resize the images, training runs more quickly and better aligns with the [ResNet reference paper](https://arxiv.org/abs/1512.03385)\. From the `~/examples/horovod/utils/preprocess` directory:

   ```
   python tensorflow_image_resizer.py \
             -d imagenet \
             -i [PATH TO TFRECORD TRAINING DATASET] \
             -o  [PATH TO RESIZED TFRECORD TRAINING DATASET] \
             --subset_name train \
             --num_preprocess_threads 60 \
             --num_intra_threads 2 \
             --num_inter_threads 2
   ```

## Train a ResNet\-50 ImageNet Model on a Single DLAMI<a name="tutorial-horovod-tensorflow-imagenet"></a>

**Note**  
The script in this tutorial expects the preprocessed training data to be in the `~/data/tf-imagenet/` folder\. Refer to [Prepare the ImageNet Dataset](#tutorial-horovod-tensorflow-imagenet-prep) for instructions\.
A hosts file is required\. Refer to [Configure Your Horovod Hosts File](#tutorial-horovod-tensorflow-configure) for instructions\.

**Use Horovod to Train a ResNet50 CNN on the ImageNet Dataset**

1. Navigate to the `~/examples/horovod/tensorflow` folder\. 

   ```
   cd ~/examples/horovod/tensorflow
   ```

1. Verify your configuration and set the number of GPUs to use in training\. First, review the `hosts` that is in the same folder as the scripts\. This file must be updated if you use an instance with fewer than 8 GPUs\. By default it says `localhost slots=8`\. Update the number 8 to be the number of GPUs you want to use\.

1. A shell script is provided that takes the number of GPUs you plan to use as its only parameter\. Run this script to start training\. The example below uses 4 for four GPUs\.

   ```
   ./train.sh 4
   ```

1. It takes several hours to finish\. It uses `mpirun` to distribute the training across your GPUs\. 

## Train a ResNet\-50 ImageNet Model on a Cluster of DLAMIs<a name="tutorial-horovod-tensorflow-imagenet-distributed"></a>

**Note**  
The script in this tutorial expects the preprocessed training data to be in the `~/data/tf-imagenet/` folder\. Refer to [Prepare the ImageNet Dataset](#tutorial-horovod-tensorflow-imagenet-prep) for instructions\.
A hosts file is required\. Refer to [Configure Your Horovod Hosts File](#tutorial-horovod-tensorflow-configure) for instructions\.

This example walks you through training a ResNet\-50 model on a prepared dataset across multiple nodes in a cluster of DLAMIs\. 
+ For faster performance, we recommend that you have the dataset locally on each member of the cluster\.

  Use this `copyclust` function to copy data to other members\.

  ```
  function copyclust(){ while read -u 10 host; do host=${host%% slots*}; rsync -azv "$2" $host:"$3"; done 10<$1; };
  ```

 Or, if you have the files sitting in an S3 bucket, use the `runclust` function to download the files to each member directly\.

```
runclust hosts "tmux new-session -d \"export AWS_ACCESS_KEY_ID=YOUR_ACCESS_KEY && export AWS_SECRET_ACCESS_KEY=YOUR_SECRET && aws s3 sync s3://your-imagenet-bucket ~/data/tf-imagenet/ && aws s3 sync s3://your-imagenet-validation-bucket ~/data/tf-imagenet/\""
```

Using tools that let you manage multiple nodes at once is a great time\-saver\. You can either wait for each step and manage each instance separately, or use tools such as tmux or screen to let you disconnect and resume sessions\. 

 After the copying is completed, you're ready to start training\. Run the script, passing 32 as a parameter for the 32 GPUs we're using for this run\. Use tmux or similar tool if you're concerned about disconnecting and terminating your session, which would end the training run\. 

```
./train.sh 32
```

 The following output is what you see when running the training on ImageNet with 32 GPUs\. Thirty\-two GPUs take 90–110 minutes\.

```
    Step Epoch  Speed  Loss   FinLoss LR
    0   0.0   440.6  6.935  7.850 0.00100
    1   0.0  2215.4  6.923  7.837 0.00305
    50   0.3 19347.5  6.515  7.425 0.10353
    100   0.6 18631.7  6.275  7.173 0.20606
    150   1.0 19742.0  6.043  6.922 0.30860
    200   1.3 19790.7  5.730  6.586 0.41113
    250   1.6 20309.4  5.631  6.458 0.51366
    300   1.9 19943.9  5.233  6.027 0.61619
    350   2.2 19329.8  5.101  5.864 0.71872
    400   2.6 19605.4  4.787  5.519 0.82126
    ...
    13750  87.9 19398.8  0.676  1.082 0.00217
    13800  88.2 19827.5  0.662  1.067 0.00156
    13850  88.6 19986.7  0.591  0.997 0.00104
    13900  88.9 19595.1  0.598  1.003 0.00064
    13950  89.2 19721.8  0.633  1.039 0.00033
    14000  89.5 19567.8  0.567  0.973 0.00012
    14050  89.8 20902.4  0.803  1.209 0.00002
    Finished in 6004.354426383972
```

After a training run is completed, the script follows up with an evaluation run\. It runs on the leader because it runs quickly enough without having to distribute the job to the other members\. The following is the output of the evaluation run\. 

```
Horovod size: 32
Evaluating
Validation dataset size: 50000
[ip-172-31-36-75:54959] 7 more processes have sent help message help-btl-vader.txt / cma-permission-denied
[ip-172-31-36-75:54959] Set MCA parameter "orte_base_help_aggregate" to 0 to see all help / error messages
  step  epoch  top1    top5     loss   checkpoint_time(UTC)
 14075   90.0  75.716   92.91    0.97  2018-11-14 08:38:28
```

The following is an example output when this script is run with 256 GPUs where the runtime was between 14 and 15 minutes\.

```
  Step Epoch  Speed  Loss   FinLoss LR
  1400  71.6 143451.0  1.189  1.720 0.14850
  1450  74.2 142679.2  0.897  1.402 0.10283
  1500  76.7 143268.6  1.326  1.809 0.06719
  1550  79.3 142660.9  1.002  1.470 0.04059
  1600  81.8 143302.2  0.981  1.439 0.02190
  1650  84.4 144808.2  0.740  1.192 0.00987
  1700  87.0 144790.6  0.909  1.359 0.00313
  1750  89.5 143499.8  0.844  1.293 0.00026
Finished in 860.5105031204224

Finished evaluation
1759   90.0  75.086   92.47    0.99  2018-11-20 07:18:18
```

## Troubleshooting<a name="tutorial-horovod-troubleshooting"></a>

The following command may help get past errors that come up when you experiment with Horovod\. 
+  If the training crashes for some reason, mpirun may fail to clean up all the python processes on each machine\. In that case before you start the next job kill the python processes on all machines as follows: 

  ```
  runclust hosts "pkill -9 python"
  ```
+ If the process finishes abruptly without error, try deleting your log folder\.

  ```
  runclust hosts "rm -rf ~/imagenet_resnet/"
  ```
+ If other unexplained issues pop up, check your disk space\. If you're out, try removing the logs folder since that is full of checkpoints and data\. You can also increase the size of the volumes for each member\.

  ```
  runclust hosts "df /"
  ```
+ As a last resort you can also try rebooting\.

  ```
  runclust hosts "sudo reboot"
  ```

You may receive the following error code if you try to use TensorFlow with Horovod on an unsupported instance type: 

```
---------------------------------------------------------------------------
NotFoundError Traceback (most recent call last)
<ipython-input-3-e90ed6cabab4> in <module>()
----> 1 import horovod.tensorflow as hvd

~/anaconda3/envs/tensorflow_p36/lib/python3.6/site-packages/horovod/tensorflow/__init__.py in <module>()
** *34* check_extension('horovod.tensorflow', 'HOROVOD_WITH_TENSORFLOW', __file__, 'mpi_lib')
** *35* 
---> 36 from horovod.tensorflow.mpi_ops import allgather, broadcast, _allreduce
** *37* from horovod.tensorflow.mpi_ops import init, shutdown
** *38* from horovod.tensorflow.mpi_ops import size, local_size, rank, local_rank

~/anaconda3/envs/tensorflow_p36/lib/python3.6/site-packages/horovod/tensorflow/mpi_ops.py in <module>()
** *56* 
** *57* MPI_LIB = _load_library('mpi_lib' + get_ext_suffix(),
---> 58 ['HorovodAllgather', 'HorovodAllreduce'])
** *59* 
** *60* _basics = _HorovodBasics(__file__, 'mpi_lib')

~/anaconda3/envs/tensorflow_p36/lib/python3.6/site-packages/horovod/tensorflow/mpi_ops.py in _load_library(name, op_list)
** *43* """
** *44* filename = resource_loader.get_path_to_datafile(name)
---> 45 library = load_library.load_op_library(filename)
** *46* for expected_op in (op_list or []):
** *47* for lib_op in library.OP_LIST.op:

~/anaconda3/envs/tensorflow_p36/lib/python3.6/site-packages/tensorflow/python/framework/load_library.py in load_op_library(library_filename)
** *59* RuntimeError: when unable to load the library or get the python wrappers.
** *60* """
---> 61 lib_handle = py_tf.TF_LoadLibrary(library_filename)
** *62* 
** *63* op_list_str = py_tf.TF_GetOpList(lib_handle)

NotFoundError: /home/ubuntu/anaconda3/envs/tensorflow_p36/lib/python3.6/site-packages/horovod/tensorflow/mpi_lib.cpython-36m-x86_64-linux-gnu.so: undefined symbol: _ZN10tensorflow14kernel_factory17OpKernelRegistrar12InitInternalEPKNS_9KernelDefEN4absl11string_viewEPFPNS_8OpKernelEPNS_20OpKernelConstructionEE
```

## More Info<a name="tutorial-horovod-project"></a>

For utilities and examples, see the `~/examples/horovod` folder in the home directory of the DLAMI\. 

For even more tutorials and examples, see the [Horovod GitHub project](https://github.com/uber/horovod)\.