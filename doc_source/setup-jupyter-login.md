# Test by Logging in to the Jupyter notebook server<a name="setup-jupyter-login"></a>

Now you are ready to log in to the Jupyter notebook server\.

**Your next step is to test the connection to the server through your browser\.**

1. In the address bar of your browser, type the following URL\.
   + For macOS and Linux clients, type the following URL\.

     ```
     http://127.0.0.1:8157
     ```
   + For Windows clients, use localhost or the public DNS name of the Amazon EC2 instance and the Jupyter port number\. The Jupyter port is typically 8888\.

     ```
     http://127.0.0.1:8888
     ```
**Note**  
`https` works only if you went through the extra step of [Custom SSL and Server Configuration](setup-jupyter-config.md)\. These examples use `http` instead, but once your SSL cert is configured, you can switch to `https`\. 

1. If the connection is successful, you see the Jupyter notebook server webpage\. At this point, you maybe asked for a password or token\. If you did a simple setup without configuring Jupyter, the token is displayed in the terminal window where you launched the server\. Look for something like: 

   `Copy/paste this URL into your browser when you connect for the first time, to login with a token: http://localhost:8888/?token=0d3f35c9e404882eaaca6e15efdccbcd9a977fee4a8bc083`

   Copy the token \(the long series of digits\), in this case it would be `0d3f35c9e404882eaaca6e15efdccbcd9a977fee4a8bc083`, and use that to access your Jupyter notebook server\.

   If, on the other hand, you edited the Jupyter config with a [Custom SSL and Server Configuration](setup-jupyter-config.md), type the password that you created when you configured the Jupyter notebook server\.  
![\[login screen\]](http://docs.aws.amazon.com/dlami/latest/devguide/images/jupyter-login.png)
**Note**  
The Jupyter login screen will ask for a token by default\. However, if you set up a password, the prompt will ask for a password instead\.

   Now you have access to the Jupyter notebook server that is running on the DLAMI\. You can create new notebooks or run the provided [Tutorials](tutorials.md)\.