# Running Jupyter Notebook Tutorials<a name="tutorial-jupyter"></a>

Tutorials and examples ship with each of the deep learning projects' source and in most cases they will run on any DLAMI\. If you chose the [Deep Learning AMI with Conda](overview-conda.md), you get the added benefit of a few hand\-picked tutorials already set up and ready to try out\. 

To run the Jupyter notebook tutorials installed on the DLAMI, you will need to [Set up a Jupyter Notebook Server](setup-jupyter.md)\.

Once the Jupyter server is running, you can run the tutorials through your web browser\. If you are running the Deep Learning AMI with Conda or if you have set up Python environments, you can switch Python kernels from the Jupyter notebook interface\. Select the appropriate kernel before trying to run a framework\-specific tutorial\. Further examples of this are provided for users of the Deep Learning AMI with Conda\.

As a quick recap of how to start up Jupyter and connect, run the following on your instance\.

```
$ source activate python3
$ jupyter notebook
```

Then run the following locally on your macOS or Linux client\. For Windows, see detailed instructions at [Set up PuTTY](setup-jupyter-configure-client-windows.md#setup-jupyter-win)\.

```
$ ssh -i ~/mykeypair.pem -L 8157:127.0.0.1:8888 ubuntu@ec2-###-##-##-###.compute-1.amazonaws.com
```

If you want to try other tutorials, just download them to this folder and run them from Jupyter\.

**Note**  
Many tutorials require additional Python modules that may not be set up on your DLAMI\. If you get an error like `"xyz module not found"`, log in to the DLAMI, activate the environment as described above, then install the necessary modules\. 

**Tip**  
Deep learning tutorials and examples often rely on one or more GPUs\. If your instance type doesn't have a GPU, you may need to change some of the example's code to get it to run\.

## Navigating the Installed Tutorials<a name="tutorial-jupyter-nav"></a>

Once you're logged in to the Jupyter server and can see the tutorials directory, you will be presented with folders of tutorials by each framework name\. If you don't see a framework listed, then tutorials are not available for that framework on your current DLAMI\. Click on the name of the framework to see the listed tutorials, then click a tutorial to launch it\.

The first time you run a notebook on the Deep Learning AMI with Conda, it will want to know which environment you would like to use\. It will prompt you to select from a list\. Each environment is named according to this pattern:

`Environment (conda_framework_python-version)`

For example, you might see `Environment (conda_mxnet_p36)`, which signifies that the environment has MXNet and Python 3\. The other variation of this would be `Environment (conda_mxnet_p27)`, which signifies that the environment has MXNet and Python 2\.

**Tip**  
If you're concerned about which version of CUDA is active, one way to see it is in the MOTD when you first log in to the DLAMI\. 

## Switching Environments with Jupyter<a name="tutorial-jupyter-switching"></a>

If you decide to try a tutorial for a different framework, be sure to verify the currently running kernel\. This info can be seen in the upper right of the Jupyter interface, just below the logout button\. You can change the kernel on any open notebook by clicking the Jupyter menu item **Kernel**, then **Change Kernel**, and then clicking the environment that suits the notebook you're running\.

At this point you'll need to rerun any cells because a change in the kernel will erase the state of anything you've run previously\.

**Tip**  
Switching between frameworks can be fun and educational, however you can run out of memory\. If you start running into errors, look at the terminal window that has the Jupyter server running\. There are helpful messages and error logging here, and you may see an out\-of\-memory error\. To fix this, you can go to the home page of your Jupyter server, click the **Running** tab, then click **Shutdown** for each of the tutorials that are probably still running in the background and eating up all of your memory\.

**Next Up**  
For more examples and sample code from each framework, click **Next** or continue to [Apache MXNet \(incubating\)](tutorial-mxnet.md)\.