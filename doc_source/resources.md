# Resources and Support<a name="resources"></a>

**Topics**
+ [Forums](#resources-forums)
+ [Related Blog Posts](#resources-blogs)
+ [FAQ](#faq)

## Forums<a name="resources-forums"></a>
+ [Forum: AWS Deep Learning AMIs](https://forums.aws.amazon.com/forum.jspa?forumID=263)

## Related Blog Posts<a name="resources-blogs"></a>
+ [Updated List of Articles Related to Deep Learning AMIs](https://aws.amazon.com/blogs/ai/category/artificial-intelligence/aws-deep-learning-amis/)
+ [Launch a AWS Deep Learning AMI \(in 10 minutes\)](https://aws.amazon.com/getting-started/tutorials/get-started-dlami/)
+ [Faster Training with Optimized TensorFlow 1\.6 on Amazon EC2 C5 and P3 Instances](https://aws.amazon.com/blogs/machine-learning/faster-training-with-optimized-tensorflow-1-6-on-amazon-ec2-c5-and-p3-instances/)
+ [New AWS Deep Learning AMIs for Machine Learning Practitioners](https://aws.amazon.com/blogs/ai/new-aws-deep-learning-amis-for-machine-learning-practitioners/)
+ [New Training Courses Available: Introduction to Machine Learning & Deep Learning on AWS](https://aws.amazon.com/blogs/apn/new-training-courses-available-introduction-to-machine-learning-deep-learning-on-aws/)
+ [Journey into Deep Learning with AWS](https://aws.amazon.com/blogs/aws/journey-into-deep-learning-with-aws/)

## FAQ<a name="faq"></a>
+ **Q\.** How do I keep track of product announcements related to DLAMI?

  Here are two suggestions for this: 
  + Bookmark this blog category, "AWS Deep Learning AMIs" found here: [Updated List of Articles Related to Deep Learning AMIs](https://aws.amazon.com/blogs/ai/category/artificial-intelligence/aws-deep-learning-amis/)\.
  + "Watch" the [Forum: AWS Deep Learning AMIs](https://forums.aws.amazon.com/forum.jspa?forumID=263)
+ **Q\.** Are the NVIDIA drivers and CUDA installed?

  Yes\. Some DLAMIs have different versions\. The [Deep Learning AMI with Conda](overview-conda.md) has the most recent versions of any DLAMI\. This is covered in more detail in [CUDA Installations and Framework Bindings](overview-cuda.md)\. You can also refer to the specific AMI's detail page on the marketplace to confirm what is installed\.
+ **Q\.** Is cuDNN installed?

  Yes\.
+ **Q\.** How do I see that the GPUs are detected and their current status?

  Run `nvidia-smi`\. This will show one or more GPUs, depending on the instance type, along with their current memory consumption\.
+ **Q\.** Are virtual environments set up for me?

  Yes, but only on the [Deep Learning AMI with Conda](overview-conda.md)\.
+ **Q\.** What version of Python is installed?

  Each DLAMI has both Python 2 and 3\. The [Deep Learning AMI with Conda](overview-conda.md) have environments for both versions for each framework\. The [Deep Learning AMI with Source Code](overview-source.md) has the deep learning frameworks installed into specific Python versions, denoted with "2" or "3" in the folder names\.
+ **Q\.** Is Keras installed?

  This depends on the AMI\. The [Deep Learning AMI with Conda](overview-conda.md) has Keras available as a front end for each framework\. The version of Keras depends on the framework's support for it\.
+ **Q\.** Is it free?

  All of the DLAMIs are free\. However, depending on the instance type you choose, the instance may not be free\. See [Pricing for the DLAMI](pricing.md) for more info\.
+ **Q\.** I'm getting CUDA errors or GPU\-related messages from my framework\. What's wrong?

  Check what instance type you used\. It needs to have a GPU for many examples and tutorials to work\. If running `nvidia-smi` shows no GPU, then you need to spin up another DLAMI using an instance with one or more GPUs\. See [Selecting the Instance Type for DLAMI](instance-select.md) for more info\.
+ **Q\.** Can I use Docker?

  Docker is not installed, but you can install it and use it\. Note that you will want to use [nvidia\-docker](https://github.com/NVIDIA/nvidia-docker) on GPU instances to make use of the GPU\. In this situation, a [AWS Deep Learning AMI, Ubuntu Versions](ubuntu.md) is your best choice, as there are currently some incompatibilities with [nvidia\-docker](https://github.com/NVIDIA/nvidia-docker) and the Deep Learning AMI \(Amazon Linux\)\.
+ **Q\.** What regions are Linux DLAMIs available in?    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dlami/latest/devguide/resources.html)
+ **Q\.** What regions are Windows DLAMIs available in?    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/dlami/latest/devguide/resources.html)