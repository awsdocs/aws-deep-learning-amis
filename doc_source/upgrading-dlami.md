# Upgrading to a New DLAMI Version<a name="upgrading-dlami"></a>

DLAMI's system images are updated on a regular basis to take advantage of new deep learning framework releases, CUDA and other software updates, and performance tuning\. If you have been using a DLAMI for some time and want to take advantage of an update, you would need to launch a new instance\. You would also have to manually transfer any datasets, checkpoints, or other valuable data\. Instead, you may use Amazon EBS to retain your data and attach it to a new DLAMI\. In this way, you can upgrade often, while minimizing the time it takes to transition your data\.

**Note**  
When attaching and moving Amazon EBS volumes between DLAMIs, you must have both the DLAMIs and the new volume in the same Availability Zone\.

1. Use the Amazon EC2console to create a new Amazon EBS volume\. For detailed directions, see [Creating an Amazon EBS Volume](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-creating-volume.html)\.

1. Attach your newly created Amazon EBS volume to your existing DLAMI\. For detailed directions, see [Attaching an Amazon EBS Volume](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-attaching-volume.html)\.

1. Transfer your data, such as datasets, checkpoints, and configuration files\.

1. Launch a DLAMI\. For detailed directions, see [Launching and Configuring a DLAMI](launch-config.md)\.

1. Detach the Amazon EBS volume from your old DLAMI\. For detailed directions, see [Detaching an Amazon EBS Volume](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-detaching-volume.html)\.

1. Attach the Amazon EBS volume to your new DLAMI\. Follow the instructions from the Step 2 to attach the volume\.

1. After you verify that your data is available on your new DLAMI, stop and terminate your old DLAMI\. For detailed clean\-up instructions, see [Clean Up](launch-config-cleanup.md)\.