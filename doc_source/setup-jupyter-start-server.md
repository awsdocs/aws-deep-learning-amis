# Start the Jupyter notebook server<a name="setup-jupyter-start-server"></a>

Now you can fire up the Jupyter server by logging in to the instance and running the following command that uses the SSL certificate you created in the previous step\.

```
$ jupyter notebook --certfile=~/ssl/mycert.pem --keyfile ~/ssl/mykey.key
```

With the server started, you can now connect to it via an SSH tunnel from your client computer\. When the server runs, you will see some output from Jupyter confirming that the server is running\. At this point, ignore the callout that you can access the server via a localhost URL, because that won't work until you create the tunnel\.

**Note**  
Jupyter will handle switching environments for you when you switch frameworks using the Jupyter web interface\. More info on this can be found in [Switching Environments with Jupyter](tutorial-jupyter.md#tutorial-jupyter-switching)\.

**Next Step**  
[Configure the Client to Connect to the Jupyter Server ](setup-jupyter-configure-client.md)