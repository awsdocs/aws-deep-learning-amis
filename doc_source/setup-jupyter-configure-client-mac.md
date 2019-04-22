# Configure a MacOS Client<a name="setup-jupyter-configure-client-mac"></a>

1. Open a terminal\.

1. Run the following command to forward all requests on local port 8157 to port 8888 on your remote Amazon EC2 instance\. Update the command by replacing *ec2\-\#\#\#\-\#\#\-\#\#\-\#\#\#\.compute\-1\.amazonaws\.com* with the public DNS name of your EC2 instance, or just use the public IP address\. Note, for an Amazon Linux AMI, the user name is ec2\-user\.

   ```
   $ ssh -i ~/mykeypair.pem -L 8157:127.0.0.1:8888 ubuntu@ec2-###-##-##-###.compute-1.amazonaws.com
   ```

   This command opens a tunnel between your client and the remote EC2 instance that is running the Jupyter notebook server\. After running the command, you can access the Jupyter notebook server at **http://127\.0\.0\.1:8157**\.
**Note**  
`https` works only if you went through the extra step of [Custom SSL and Server Configuration](setup-jupyter-config.md)\. These examples use `http` instead, but once your SSL cert is configured, you can switch to `https`\. 

**Next Step**  
[Test by Logging in to the Jupyter notebook server](setup-jupyter-login.md)