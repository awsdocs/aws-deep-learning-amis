# Logging and Monitoring in AWS Deep Learning AMI<a name="logging-and-monitoring"></a>

Your AWS Deep Learning AMI instance comes with several GPU monitoring tools including a utility that reports GPU usage statistics to Amazon CloudWatch\. For more information, see [GPU Monitoring and Optimization](https://docs.aws.amazon.com/dlami/latest/devguide/tutorial-gpu.html) and [Monitoring Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring_ec2.html)\. 

## Usage Tracking<a name="logging-and-monitoring-opt-out"></a>

The following AWS Deep Learning AMI operating system distributions include code that allows AWS to collect instance type, instance ID, DLAMI type, and OS information\. No information on the commands used within the DLAMI is collected or retained\. No other information about the DLAMI is collected or retained\.
+ Ubuntu 16\.04
+ Ubuntu 18\.04
+ Ubuntu 20\.04
+ Amazon Linux 2

To opt out of usage tracking for your DLAMI, add a tag to your Amazon EC2 instance during launch\. The tag should use the key `OPT_OUT_TRACKING` with the associated value set to `true`\. For more information, see [Tag your Amazon EC2 resources\.](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)