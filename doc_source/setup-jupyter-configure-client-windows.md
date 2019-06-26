# Configure a Windows Client<a name="setup-jupyter-configure-client-windows"></a>

## Prepare<a name="setup-jupyter-configure-client-prepare"></a>

Be sure you have the following information, which you need to set up the SSH tunnel:
+ The public DNS name of your Amazon EC2 instance\. You can find the public DNS name in the EC2 console\. 
+ The key pair for the private key file\. For more information about accessing your key pair, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide for Linux Instances*\. 

## Using Jupyter Notebooks from a Windows Client<a name="setup-jupyter-win"></a>

Refer to these guides on connecting to your Amazon EC2 instance from a Windows client\.

1. [Troubleshooting Connecting to Your Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/TroubleshootingInstancesConnecting.html)

1. [Connecting to Your Linux Instance from Windows Using PuTTY](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html)

To create a tunnel to a running Jupyter server, a recommended approach is to install Git Bash on your Windows client, then follow the Linux/macOS client instructions\. However, you may use whatever approach you want for opening an SSH tunnel with port mapping\. Refer to [Jupyter's documentation](https://jupyter-notebook.readthedocs.io/en/stable/public_server.html) for further information\.

**Next Step**  
[Configure a Linux or macOS Client](setup-jupyter-configure-client-linux.md)