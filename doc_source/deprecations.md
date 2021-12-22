# Deprecations for DLAMI<a name="deprecations"></a>

The following table lists information on deprecated features in the AWS Deep Learning AMI\.


|  Deprecated Feature  |  Deprecation Date  |  Deprecation Notice  | 
| --- | --- | --- | 
|  Ubuntu 16\.04  |  10/07/2021  |  Ubuntu Linux 16\.04 LTS reached the end of its five\-year LTS window on April 30, 2021 and is no longer supported by its vendor\. There are no longer updates to the Deep Learning Base AMI \(Ubuntu 16\.04\) in new releases as of October 2021\. Previous releases will continue to be available\.  | 
|  Amazon Linux  |  10/07/2021  |  Amazon Linux is [end\-of\-life](http://aws.amazon.com/blogs/aws/update-on-amazon-linux-ami-end-of-life/) as of December 2020\. There are no longer updates to the Deep Learning AMI \(Amazon Linux\) in new releases as of October 2021\. Previous releases of the Deep Learning AMI \(Amazon Linux\) will continue to be available\.  | 
|  Chainer  |  07/01/2020  |  Chainer has announced [the end of major releases](https://chainer.org/announcement/2019/12/05/released-v7.html) as of December, 2019\. Consequently, we will no longer include Chainer Conda environments on the DLAMI starting July 2020\. Previous releases of the DLAMI that contain these environments will continue to be available\. We will provide updates to these environments only if there are security fixes published by the open source community for these frameworks\.   | 
|  Python 3\.6  |  06/15/2020  |  Due to customer requests, we are moving to Python 3\.7 for new TF/MX/PT releases\.  | 
|  Python 2  |  01/01/2020  |   The Python open source community has officially ended support for Python 2\.   The TensorFlow, PyTorch, and MXNet communities have also announced that TensorFlow 1\.15, TensorFlow 2\.1, PyTorch 1\.4, and MXNet 1\.6\.0 releases will be the last ones supporting Python 2\.  | 