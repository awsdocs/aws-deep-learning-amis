# TensorFlow with Horovod<a name="tutorial-horovod"></a>

This tutorial shows how to use TensorFlow with Horovod on a Deep Learning AMI with Conda\. Horovod is pre\-installed in the Conda environments for TensorFlow\. The Python 3 environment is recommended\. 

**Note**  
Only P3\.\*, P2\.\*, and G3\.\* instance types are supported\.

**To activate TensorFlow and Test Horovod on the DLAMI with Conda**

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

   The following should appear on your screen \(possibly after a few warning messages\)\.

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

## Train ImageNet with Horovod<a name="tutorial-horovod-imagenet"></a>

In this section, you will download the ImageNet dataset, then generate a TFRecord\-format dataset from the raw dataset\. A set of preprocessing scripts is provided on the DLAMI for the ImageNet dataset that you can use for either ImageNet or as a template for another dataset\. The main training scripts that are configured for ImageNet are also provided\. The following tutorial assumes that you have launched a DLAMI with an EC2 instance with 8 GPUs\. We recommend the p3\.16xlarge instance type\. 

In the `~/examples/horovod` directory on your DLAMI you will find the following scripts: 
+ `utils/preprocess_imagenet.py` \- Use this to convert the raw ImageNet dataset to the `TFRecord` format\.
+ `utils/tensorflow_image_resizer.py` \- Use this to resize the `TFRecord` dataset as recommended for ImageNet training\.
+ `cnn/aws_tf_hvd_cnn.py` \- Use this with `mpirun` to train a CNN with Horovod on the pre\-processed ImageNet dataset\.
+ `cnn/aws_tf_cnn.py` \- Use this to train a CNN without Horovod on the pre\-processed ImageNet dataset\.

**Prepare the ImageNet Dataset**

1. Visit [image\-net\.org](http://image-net.org), create an account, acquire an access key, and download the dataset\. [image\-net\.org](http://image-net.org) hosts the raw dataset\. To download it, you are required to have an ImageNet account and an access key\. The account is free, and to get the free access key you must agree to the ImageNet license\. 

1. Use the image pre\-processing script to generate a TFRecord format dataset from the raw ImageNet dataset\. From the `~/examples/horovod/utils/preprocess` directory:

   ```
   python preprocess_imagenet.py \
            --local_scratch_dir=[YOUR DIRECTORY] \
            --imagenet_username=[imagenet account] \
            --imagenet_access_key=[imagenet access key]
   ```

1. Use the image resizing script\. If you resize the images, training runs more quickly and better align with the [ResNet reference paper](https://arxiv.org/abs/1512.03385)\. From the `~/examples/horovod/utils/preprocess` directory:

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

**Use Horovod to Train a ResNet50 CNN on the ImageNet Dataset**

1. Navigate to the `~/examples/horovod/cnn` folder\. 

   ```
   cd ~/examples/horovod/cnn
   ```

1. Verify your configuration and set the number of GPUs to use in training\. First, review the `hostfile` that is in the same folder as the scripts\. This file must be updated if you use an instance with fewer than 8 GPUs\. By default it says `localhost slots=8`\. Update the number 8 to be the number of GPUs you want to use\.

1. Run the training script\. It takes several hours to finish\. It uses `mpirun` to distribute the training across your GPUs\. 

   Using `--save_interval` is optional\. This saves a checkpoint every hour \(3,600 seconds\)\. Saving checkpoints slows down training, but provides flexibility in performance evaluation\.
**Note**  
The script expects the pre\-processed training data to be in the `train` folder\. Change this value to the location of your training data\.
   + If you are using Amazon Linux or Ubuntu with the latest version of the DLAMI \(version 13 or greater\), you must use `mpirun -np 8` where 8 is the number of GPUs to use:

     ```
       mpirun -np 8 \
       --hostfile hostfile --bind-to none --map-by slot -x NCCL_DEBUG=INFO \
       -x NCCL_MIN_NRINGS=4 -x LD_LIBRARY_PATH -x PATH  -mca pml ob1 -mca btl ^openib \
       python aws_tf_hvd_cnn.py --batch_size=256 --num_epochs=90 --fp16 \
       --data_dir train --model resnet50 --log_dir results --display_every 100 --save_interval=3600
     ```
   + If you are using an older DLAMI with Ubuntu \(version 12 or less\), you must use `mpirun.openmpi -np 8` where 8 is the number of GPUs to use:

     ```
     mpirun.openmpi -np 8 \
     --hostfile hostfile --bind-to none --map-by slot -x NCCL_DEBUG=INFO \
     -x NCCL_MIN_NRINGS=4 -x LD_LIBRARY_PATH -x PATH  -mca pml ob1 -mca btl ^openib \
     python aws_tf_hvd_cnn.py --batch_size=256 --num_epochs=90 --fp16 \
     --data_dir train --model resnet50 --log_dir results --display_every 100 --save_interval=3600
     ```

1. After training is finished, run the evaluation script\.

   ```
   python -u aws_tf_hvd_cnn.py --batch_size=256 \
          --num_epochs=90 --data_dir [PATH TO TFRECORD VALIDAGTION DATASET] \
          --model resnet50 --log_dir [PATH TO RESULT] \
          --display_every 100  \
          --eval
   ```

You can also try out the training scripts without Horovod support\.

**Train a ResNet50 CNN on the ImageNet Dataset Without Horovod**

1. Navigate to the `~/examples/horovod/cnn` folder, then make sure that you have a folder for logging of the results\. 

   ```
   cd ~/examples/horovod/cnn
   mkdir results
   ```

1. Run the training script configured without Horovod\. It takes several hours to finish\.
**Note**  
The script expects the pre\-processed training data to be in the `train` folder\. Change this value to the location of your training data\.

   ```
   python ./cnn/aws_tf_cnn.py
               --num_epochs=90 \
               --batch_size=256 \
               --display_every 100 \
               --data_dir=[PATH TO TFRECORD TRAINING DATASET] \
               --log_dir=[PATH TO CHECKPOINT DIR] \
               --model=resnet50 \
               --num_gpus=8 \
               --fp16
   ```

1. After training is finished, run the script with the evaluation setting\.

   ```
   python ./cnn/aws_tf_cnn.py
               --num_epochs=90 \
               --batch_size=256 \
               --display_every 100 \
               --data_dir=[PATH TO TFRECORD VALIDAGTION DATASET] \
               --log_dir=[PATH TO CHECKPOINT DIR] \
               --model=resnet50 \
               --num_gpus=8 \
               --fp16 \
               --eval
   ```

If you compared the Horovod training to the non\-Horovod training, you see the improvement in speeds that Horovod brings to training a large dataset with TensorFlow\.

## More Info<a name="tutorial-horovod-project"></a>

[TensorFlow with Horovod](activate-horovod.md)

For tutorials, see the `examples/horovod` folder in the home directory of the DLAMI\. 

For even more tutorials and examples, see the [Horovod GitHub project](https://github.com/uber/horovod)\.