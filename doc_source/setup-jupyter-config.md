# Custom SSL and Server Configuration<a name="setup-jupyter-config"></a>

Here we set up a Jupyter notebook with SSL and a config file\.

Connect to the Amazon EC2 instance, and then complete the following procedure\.

**Configure the Jupyter server**

1. Create an SSL certificate\.

   ```
   $ cd
   $ mkdir ssl
   $ cd ssl
   $ sudo openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout "cert.key" -out "cert.pem" -batch
   ```

1. Create a password\. You use this password to log in to the Jupyter notebook server from your client so you can access notebooks on the server\.

   1. Open the iPython terminal\.

      ```
      $ ipython
      ```

      At the iPython prompt, run the `passwd()`command to set the password\.

      ```
      iPythonPrompt> from IPython.lib import passwd
      iPythonPrompt> passwd()
      ```

      You get the password hash \(For example, `sha1:examplefc216:3a35a98ed...`\)\.

   1. Record the password hash\. 

   1. Exit the iPython terminal\.

      ```
      $ exit
      ```

1. Edit the Jupyter configuration file\. 

   Find `jupyter_notebook_config.py` in the `~/.jupyter` directory\.

1. Update the configuration file to store your password and SSL certificate information\.

   1. Open the \.config file\.

      ```
      $ vi ~/.jupyter/jupyter_notebook_config.py
      ```

   1. Paste the following text at the end of the file\. You will need to provide your password hash\.

      ```
      c = get_config()  # Get the config object.
      c.NotebookApp.certfile = u'/home/ubuntu/ssl/cert.pem' # path to the certificate we generated
      c.NotebookApp.keyfile = u'/home/ubuntu/ssl/cert.key' # path to the certificate key we generated
      c.NotebookApp.ip = '*'  # Serve notebooks locally.
      c.NotebookApp.open_browser = False  # Do not open a browser window by default when using notebooks.
      c.NotebookApp.password = 'sha1:fc216:3a35a98ed980b9...'
      ```
**Note**  
If you're using a DLAMI that doesn't have a default Jupyter config file for you, you need to create one\.  

      ```
      $ jupyter notebook --generate-config 
      ```
Once created, follow the same steps for updating the config with your SSL info\.

      This completes Jupyter server configuration\.

**Next Step**  
[Start the Jupyter notebook server](setup-jupyter-start-server.md)