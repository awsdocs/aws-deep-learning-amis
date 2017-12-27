# Configure Jupyter Notebook on Your DLAMI<a name="setup-jupyter-configure-server"></a>

To use Jupyter, you need to open port 8888 \(or one of your choosing\) on your instance's firewall\. You also can set up an SSL certificate and default password, but this is optional\. Once the port is open you'll launch the server, then you'll SSH to your server and create a tunnel for you to access Jupyter's web interface\. The following steps walk you through this process\.

Open your EC2 dashboard and choose **Security Groups** on the EC2 navigation bar in the **Network & Security** section\. On this page, there will be one or more security groups in the list\. Find the most recent one \(the description has a timestamp in it\), select it, choose the **Inbound** tab, and then click **Edit**\. Then click **Add Rule**\. This adds a new row\. Fill in the fields with the following information: 

**Type** : Custom **TCP Rule**

**Protocol**: **TCP**

**Port Range**: **8888**

**Source**: **Anywhere** \(**0\.0\.0\.0/0,::/0**\)

**Tip**  
To make Jupyter run a little more seamlessly, you can modify the Jupyter configuration file\. In the configuration file, you set some of the values to use for web authentication, including the SSL certificate file path, and a password\. You can go through [Custom SSL and Server Configuration](setup-jupyter-config.md) now or at a later time\. 

**Next Step**  
[Start the Jupyter notebook server](setup-jupyter-start-server.md)