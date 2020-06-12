# Secure Your Jupyter Server<a name="setup-jupyter-config"></a>

Here we set up Jupyter with SSL and a custom password\.

Connect to the Amazon EC2 instance, and then complete the following procedure\.

**Configure the Jupyter server**

1. Jupyter provides a password utility\. Run the following command and enter your preferred password at the prompt\.

   ```
   $ jupyter notebook password
   ```

   The output will look something like this:

   ```
   Enter password:
   Verify password:
   [NotebookPasswordApp] Wrote hashed password to /home/ubuntu/.jupyter/jupyter_notebook_config.json
   ```

1. Create a self\-signed SSL certificate\. Follow the prompts to fill out your locality as you see fit\. You must enter `.` if you wish to leave a prompt blank\. Your answers will not impact the functionality of the certificate\.

   ```
   $ cd ~
   $ mkdir ssl
   $ cd ssl
   $ openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout mykey.key -out mycert.pem
   ```

**Note**  
You might be interested in creating a regular SSL certificate that is third party signed and does not cause the browser to give you a security warning\. This process is much more involved\. Visit [Jupyter's documention](https://jupyter-notebook.readthedocs.io/en/stable/public_server.html#using-let-s-encrypt) for more information\.

**Next Step**  
[Start the Jupyter notebook server](setup-jupyter-start-server.md)