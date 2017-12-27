# Configure a Linux Client<a name="setup-jupyter-configure-client-linux"></a>

1. Open a terminal\.

1. Run the following command to forward all requests on local port 8157 to port 8888 on your remote Amazon EC2 instance\. Update the command by replacing *ec2\-\#\#\#\-\#\#\-\#\#\-\#\#\#\.compute\-1\.amazonaws\.com* with the public DNS name of your EC2 instance\.

   ```
   $ ssh -i ~/mykeypair.pem -L 8157:127.0.0.1:8888 ubuntu@ec2-###-##-##-###.compute-1.amazonaws.com
   ```

   This command opens a tunnel between your client and the remote EC2 instance that is running the Jupyter notebook server\. After running the command, you can access the Jupyter notebook server at **https://127\.0\.0\.1:8157**\.

**Next Step**  
[Test by Logging in to the Jupyter notebook server](setup-jupyter-login.md)