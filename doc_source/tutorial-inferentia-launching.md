# Launching a DLAMI Instance with AWS Neuron<a name="tutorial-inferentia-launching"></a>

 The latest DLAMI is ready to use with AWS Inferentia and comes with the AWS Neuron API package\. To launch a DLAMI instance, see [Launching and Configuring a DLAMI\.](https://docs.aws.amazon.com/dlami/latest/devguide/launch-config.html) After you have a DLAMI, use the steps here to ensure that your AWS Inferentia chip and AWS Neuron resources are active\.

**Topics**
+ [Verify Your Instance](#tutorial-inferentia-launching-verify)
+ [Identifying AWS Inferentia Devices](#tutorial-inferentia-launching-identify)
+ [View Resource Usage](#tutorial-inferentia-launching-resource-usage)
+ [Using Neuron Monitor \(neuron\-monitor\)](#tutorial-inferentia-launching-neuron-monitor)
+ [Upgrading Neuron Software](#tutorial-inferentia-launching-upgrade)

## Verify Your Instance<a name="tutorial-inferentia-launching-verify"></a>

 Before using your instance, verify that it's properly setup and configured with Neuron\. 

## Identifying AWS Inferentia Devices<a name="tutorial-inferentia-launching-identify"></a>

 To identify the number of Inferentia devices on your instance, use the following command: 

```
neuron-ls
```

 If your instance has Inferentia devices attached to it, your output will look similar to the following: 

```
+--------+--------+--------+-----------+--------------+
| NEURON | NEURON | NEURON | CONNECTED |     PCI      |
| DEVICE | CORES  | MEMORY |  DEVICES  |     BDF      |
+--------+--------+--------+-----------+--------------+
| 0      | 4      | 8 GB   | 1         | 0000:00:1c.0 |
| 1      | 4      | 8 GB   | 2, 0      | 0000:00:1d.0 |
| 2      | 4      | 8 GB   | 3, 1      | 0000:00:1e.0 |
| 3      | 4      | 8 GB   | 2         | 0000:00:1f.0 |
+--------+--------+--------+-----------+--------------+
```

 The supplied output is taken from an Inf1\.6xlarge instance and includes the following columns:
+ NEURON DEVICE: The logical ID assigned to the NeuronDevice\. This ID is used when configuring multiple runtimes to use different NeuronDevices\.
+ NEURON CORES: The number of NeuronCores present in the NeuronDevice\. 
+ NEURON MEMORY: The amount of DRAM memory in the NeuronDevice\.
+ CONNECTED DEVICES: Other NeuronDevices connected to the NeuronDevice\. 
+ PCI BDF: The PCI Bus Device Function \(BDF\) ID of the NeuronDevice\.

## View Resource Usage<a name="tutorial-inferentia-launching-resource-usage"></a>

 View useful information about NeuronCore and vCPU utilization, memory usage, loaded models, and Neuron applications with the `neuron-top` command\. Launching `neuron-top` with no arguments will show data for all machine learning applications that utilize NeuronCores\. 

```
neuron-top
```

 When an application is using four NeuronCores, the output should look similar to the following image: 

![\[The output of the neuron-top command, with information for one of four NeuronCores highlighted.\]](http://docs.aws.amazon.com/dlami/latest/devguide/images/neuron-top-output.png)

For more information on resources to monitor and optimize Neuron\-based inference applications, see [Neuron Tools](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/neuron-guide/neuron-tools/index.html)\.

## Using Neuron Monitor \(neuron\-monitor\)<a name="tutorial-inferentia-launching-neuron-monitor"></a>

Neuron Monitor collects metrics from the Neuron runtimes running on the system and streams the collected data to stdout in JSON format\. These metrics are organized into metric groups that you configure by providing a configuration file\. For more information on Neuron Monitor, see the [User Guide for Neuron Monitor](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/neuron-guide/neuron-tools/neuron-monitor-user-guide.html)\.

## Upgrading Neuron Software<a name="tutorial-inferentia-launching-upgrade"></a>

For information on how to update Neuron SDK software within DLAMI, see the AWS Neuron [Setup Guide](https://awsdocs-neuron.readthedocs-hosted.com/en/latest/neuron-intro/neuron-install-guide.html)\.

**Next Step**  
[Using the DLAMI with AWS Neuron](tutorial-inferentia-using.md)