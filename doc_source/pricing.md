# Pricing for the DLAMI<a name="pricing"></a>

The DLAMI is free, however, you are still responsible for Amazon EC2 or other AWS service costs\. The included deep learning frameworks are free, and each has its own open source licenses\. The GPU software from NVIDIA is free, and has its own licenses as well\. 

How is it free, but not free? What are the "Amazon EC2 or other AWS service costs"?

This is a common question\. Some instance types on Amazon EC2 are labeled as free\. These are typically the smaller instances, and it is possible to run the DLAMI on one of these free instances\. This means that it is entirely free when you only use that instance's capacity\. If you decide that you want a more powerful instance, with more CPU cores, more disk space, more RAM, and one or more GPUs, then these are most likely not in the free\-tier instance class\. This means that you will need to pay for those costs\. One way to think of it is that the software is still free, but you have to pay for the underlying hardware that you're using\.

Take a look at [Selecting the Instance Type for DLAMI](instance-select.md) for more information on what size of instance to choose and what kinds of things you can expect from each type\.

**Next Step**  
[Launching and Configuring a DLAMI](launch-config.md)