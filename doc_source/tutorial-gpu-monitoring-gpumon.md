# Monitor GPUs with CloudWatch<a name="tutorial-gpu-monitoring-gpumon"></a>

When you use your DLAMI with a GPU you might find that you are looking for ways to track its usage during training or inference\. This can be useful for optimizing your data pipeline, and tuning your deep learning network\. 

A utility called gpumon\.py is preinstalled on your DLAMI\. It integrates with CloudWatch and supports monitoring of per\-GPU usage: GPU memory, GPU temperature, and GPU Power\. The script periodically sends the monitored data to CloudWatch\. You can configure the level of granularity for data being sent to CloudWatch by changing a few settings in the script\. Before starting the script, however, you will need to setup CloudWatch to receive the metrics\. 

**How to setup and run GPU monitoring with CloudWatch**

1. Create an IAM user, or modify an existing one to have a policy for publishing the metric to CloudWatch\. If you create a new user please take note of the credentials as you will need these in the next step\. 

   The IAM policy to search for is “cloudwatch:PutMetricData”\. The policy that is added is as follows:

   ```
   {
      "Version": "2012-10-17",
      "Statement": [
           {
               "Action": [
                   "cloudwatch:PutMetricData",
                ],
                "Effect": "Allow",
                "Resource": "*"
           }
      ]
   }
   ```
**Tip**  
For more information on creating an IAM user and adding policies for CloudWatch, refer to the [ CloudWatch documentation](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/create-iam-roles-for-cloudwatch-agent.html)\.

1. On your DLAMI, run [ AWS configure](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html#cli-quick-configuration) and specify the IAM user credentials\. 

   ```
   $ aws configure
   ```

1. You might need to make some modifications to the gpumon utility before you run it\. You can find the gpumon utility and README in the following location\.

   ```
   Folder: ~/tools/GPUCloudWatchMonitor
   Files: 	~/tools/GPUCloudWatchMonitor/gpumon.py
         	~/tools/GPUCloudWatchMonitor/README
   ```

   Options:
   + Change the region in gpumon\.py if your instance is NOT in us\-east\-1\.
   + Change other parameters such as the CloudWatch `namespace` or the reporting period with `store_resolution`\.

1. Currently the script only supports Python 2\.7\. Activate your preferred framework’s Python 2\.7 environment or activate the DLAMI general Python 2\.7 environment\. 

   ```
   $ source activate python2
   ```

1. Run the gpumon utility in background\.

   ```
   (python2)$ gpumon.py &
   ```
   
   Or, to make the gpumon utility start automatically on boot, create the file `/etc/systemd/system/gpumon.service` with this content:
   
   ```
    [Unit]
    Description=gpumon
    
    [Service]
    User=ubuntu
    ExecStart=/bin/bash -c "/usr/bin/python2.7 /home/ubuntu/tools/GPUCloudWatchMonitor/gpumon.py"
    
    [Install]
    WantedBy=multi-user.target
    ```
    
    and then `sudo systemctl start gpumon` to start it, `sudo systemctl status gpumon` to verify that it started up without apparent problems, and `sudo systemctl enable gpumon` to make it start automatically at boot time.

1. Open your browser to the [https://console\.aws\.amazon\.com/cloudwatch/](https://console.aws.amazon.com/cloudwatch/) then select metric\. It will have a namespace 'DeepLearningTrain'\. 
**Tip**  
You can change the namespace by modifying gpumon\.py\. You can also modify the reporting interval by adjusting `store_resolution`\. 

The following is an example CloudWatch chart reporting on a run of gpumon\.py monitoring a training job on p2\.8xlarge instance\. 

![\[GPU monitoring on CloudWatch\]](http://docs.aws.amazon.com/dlami/latest/devguide/images/gpumon.png)

You might be interested in these other topics on GPU monitoring and optimization:
+ [Monitoring](tutorial-gpu-monitoring.md)
  + [Monitor GPUs with CloudWatch](#tutorial-gpu-monitoring-gpumon)
+ [Optimization](tutorial-gpu-opt.md)
  + [Preprocessing](tutorial-gpu-opt-preprocessing.md)
  + [Training](tutorial-gpu-opt-training.md)
