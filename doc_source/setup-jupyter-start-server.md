# Start the Jupyter notebook server<a name="setup-jupyter-start-server"></a>

Now you can fire up the server by logging in to the instance and running the following commands\.

```
$ source activate python3
$ jupyter notebook
```

If you opened a port different from 8888, update the port parameter to the port you opened\. With the server started, you can now connect to it via an SSH tunnel from your client computer\. When the server runs, you will see some output from Jupyter confirming that the server is running\. At this point, ignore the callout that you can access the server via a localhost URL, because that won't work on a cloud instance\.

**Note**  
The reason you need to activate a Python 3 environment is that Jupyter's interface is expecting Python 3 and does not fully support Python 2\. Don't worry if you want to use a framework with Python 2\. Jupyter will handle that for you when you switch frameworks using the Jupyter web interface\. More info on this can be found in [Switching Environments with Jupyter](tutorial-jupyter.md#tutorial-jupyter-switching)\.

**Next Step**  
[Configure the Client to Connect to the Jupyter Server ](setup-jupyter-configure-client.md)