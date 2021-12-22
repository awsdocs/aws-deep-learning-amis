# Tips for Software Updates<a name="updating-software"></a>

From time to time, you may want to manually update software on your DLAMI\. It is generally recommended that you use `pip` to update Python packages\. You should also use `pip` to update packages within a Conda environment on the Deep Learning AMI with Conda\. Refer to the particular framework's or software's website for upgrading and installation instructions\. 

If you are interested in running the latest main branch of a particular package, activate the appropriate environment, then add `--pre` to the end of the `pip install --upgrade` command\. For example: 

```
source activate mxnet_p36
pip install --upgrade mxnet --pre
```