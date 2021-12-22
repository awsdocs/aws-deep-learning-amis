# Set up a Jupyter Notebook Server<a name="setup-jupyter"></a>

A Jupyter notebook server enables you to create and run Jupyter notebooks from your DLAMI instance\. With Jupyter notebooks, you can conduct machine learning \(ML\) experiments for training and inference while using the AWS infrastructure and accessing packages built into the DLAMI\. For more information about Jupyter notebooks, see [the Jupyter Notebook documentation](https://jupyter-notebook.readthedocs.io/en/latest/notebook.html)\. 

To set up a Jupyter notebook server, you must:
+ Configure the Jupyter notebook server on your Amazon EC2 DLAMI instance\.
+ Configure your client so that you can connect to the Jupyter notebook server\. We provide configuration instructions for Windows, macOS, and Linux clients\.
+ Test the setup by logging in to the Jupyter notebook server\.

To complete the steps to set up a Jupyter, follow the instructions in the following topics\. Once you've set up a Jupyter notebook server, see [Running Jupyter Notebook Tutorials](tutorial-jupyter.md) for information on running the example notebooks that ship in the DLAMI\.

**Topics**
+ [Secure Your Jupyter Server](setup-jupyter-config.md)
+ [Start the Jupyter notebook server](setup-jupyter-start-server.md)
+ [Configure the Client to Connect to the Jupyter Server](setup-jupyter-configure-client.md)
+ [Test by Logging in to the Jupyter notebook server](setup-jupyter-login.md)