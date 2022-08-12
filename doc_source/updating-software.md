# Tips for Software Updates<a name="updating-software"></a>

From time to time, you may want to manually update software on your DLAMI\. It is generally recommended that you use `pip` to update Python packages\. You should also use `pip` to update packages within a Conda environment on the Deep Learning AMI with Conda\. Refer to the particular framework's or software's website for upgrading and installation instructions\. 

**Note**  
We cannot guarantee that a package update will be successful\. Attempting to update a package in an environment with incompatible dependencies can result in a failure\. In such a case, you should contact the library maintainer to see if it is possible to update the package dependencies\. Alternatively, you can attempt to modify the environment in such a way that allows the update\. However, this modification will likely mean removing or updating existing packages, which means that we can no longer guarantee stability of this environment\.

If you are interested in running the latest main branch of a particular package, activate the appropriate environment, then add `--pre` to the end of the `pip install --upgrade` command\. For example:

```
source activate mxnet_p36
pip install --upgrade mxnet --pre
```

The AWS Deep Learning AMI comes with many Conda environments and many packages preinstalled\. Due to the number of packages preinstalled, finding a set of packages that are guaranteed to be compatible is difficult\. You may see a warning "The environment is inconsistent, please check the package plan carefully"\. DLAMI ensures that all the DLAMI\-provided environments are correct, but cannot guarantee that any user installed packages will function correctly\.