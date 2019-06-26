# Configure a Linux or macOS Client<a name="setup-jupyter-configure-client-linux"></a>

1. Open a terminal\.

1. Run the following command to forward all requests on local port 8888 to port 8888 on your remote Amazon EC2 instance\. Update the command by replacing the location of your key to access the Amazon EC2 instance and the public DNS name of your Amazon EC2 instance\. Note, for an Amazon Linux AMI, the user name is `ec2-user` instead of `ubuntu`\.

   ```
   $ ssh -i ~/mykeypair.pem -N -f -L 8888:localhost:8888 ubuntu@ec2-###-##-##-###.compute-1.amazonaws.com
   ```

   This command opens a tunnel between your client and the remote Amazon EC2 instance that is running the Jupyter notebook server\.

**Next Step**  
[Test by Logging in to the Jupyter notebook server](setup-jupyter-login.md)