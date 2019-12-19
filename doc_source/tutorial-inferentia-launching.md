# Launching a DLAMI Instance with AWS Neuron<a name="tutorial-inferentia-launching"></a>

 The latest DLAMI is ready to use with AWS Inferentia and comes with the AWS Neuron API package\. To launch a DLAMI instance, see [Launching and Configuring a DLAMI\.](https://docs.aws.amazon.com/dlami/latest/devguide/launch-config.html) After you have a DLAMI, use the steps here to ensure that your AWS Inferentia chip and AWS Neuron resources are active\.

**Topics**
+ [Upgrade Neuron Package](#tutorial-inferentia-launching-upgrade)
+ [Verify Your Instance](#tutorial-inferentia-launching-verify)
+ [Identifying AWS Inferentia Devices](#tutorial-inferentia-launching-identify)
+ [nr\_hugepages](#tutorial-inferentia-launching-hugepages)
+ [Neuron Runtime Daemon \(neuron\-rtd\)](#tutorial-inferentia-launching-runtime-daemon)
+ [NeuronCore Groups](#tutorial-inferentia-launching-neuroncore-groups)
+ [Listing Models](#tutorial-inferentia-launching-listing)
+ [View Resource Usage](#tutorial-inferentia-launching-resource-usage)

## Upgrade Neuron Package<a name="tutorial-inferentia-launching-upgrade"></a>

 After you’ve connected to your Inf1 instance, use the following commands to upgrade your Neuron package\. 

 For Amazon Linux 2, run: 

```
sudo yum install aws-neuron-runtime
sudo yum install aws-neuron-tools
```

 For Ubuntu Linux, run: 

```
sudo apt-get update
sudo apt-get install aws-neuron-runtime
sudo apt-get install aws-neuron-tools
```

## Verify Your Instance<a name="tutorial-inferentia-launching-verify"></a>

 Before using your instance, verify that it's properly setup and configured with Neuron\. 

## Identifying AWS Inferentia Devices<a name="tutorial-inferentia-launching-identify"></a>

 To identify the number of Inferentia devices on your instance, use the following command: 

```
neuron-ls
```

 If your instance has Inferentia devices attached to it, your output will look similar to the following: 

```
...
+--------------+---------+--------+-----------+-----------+------+------+
|   PCI BDF    | LOGICAL | NEURON |  MEMORY   |  MEMORY   | EAST | WEST |
|              |   ID    | CORES  | CHANNEL 0 | CHANNEL 1 |      |      |
+--------------+---------+--------+-----------+-----------+------+------+
| 0000:00:1f.0 |       0 |      4 | 4096 MB   | 4096 MB   |    0 |    0 |
+--------------+---------+--------+-----------+-----------+------+------+
```

 The supplied output is taken from an inf1\.2xlarge instance\. The first column shows the PCI Bus Device Function ID\. The second column shows the logical ID assigned to the device\. This logical ID is used during Neuron Runtime Daemon \(neuron\-rtd\) configuration\. The third column shows the number of NeuronCores available\. The last two columns show the connection to any other Inferentia devices\. Because this is a single Inferentia instance, those are empty\. 

## nr\_hugepages<a name="tutorial-inferentia-launching-hugepages"></a>

 `vm.nr_hugepages` is a system wide configuration for managing huge size pages in memory in addition to the standard 4KB page size\. This configuration is important for the Neuron API because it uses huge pages\. Run the following command to see the `vm.nr_hugepages` setting for this instance: 

```
grep HugePages_Total /proc/meminfo | awk {'print $2'}
```

 The minimum `vm.nr_hugepages` requirement is 128 per AWS Inferentia device\.  

## Neuron Runtime Daemon \(neuron\-rtd\)<a name="tutorial-inferentia-launching-runtime-daemon"></a>

 When you launch any Inf1 instance, the Neuron runtime daemon \(neuron\-rtd\) should automatically start\. You can verify that neuron\-rtd is active using the following command: 

```
sudo systemctl status neuron-rtd
```

 Your output should look similar to the following: 

```
● neuron-rtd.service - Neuron Runtime Daemon
   Loaded: loaded (/usr/lib/systemd/system/neuron-rtd.service; enabled; vendor preset: disabled)
   Active: active (running) since Thu 2019-11-14 09:53:47 UTC; 3min 0s ago
 Main PID: 3351 (neuron-rtd)
    Tasks: 14
   Memory: 11.6M
   CGroup: /system.slice/neuron-rtd.service
           └─3351 /opt/aws/neuron/bin/neuron-rtd -c /opt/aws/neuron/config/neuron-rtd.config

Nov 14 09:53:16 ip-172-31-23-213.ec2.internal systemd[1]: Starting Neuron Runtime Daemon...
Nov 14 09:53:18 ip-172-31-23-213.ec2.internal neuron-rtd[3351]: [NRTD:ParseArguments] Using all the BDFs in the ...on!
Nov 14 09:53:18 ip-172-31-23-213.ec2.internal nrtd[3351]: [NRTD:krtd_main] krt build using:1.0.3952.0
Nov 14 09:53:18 ip-172-31-23-213.ec2.internal nrtd[3351]: [TDRV:reset_mla] Resetting 0000:00:1f.0
Nov 14 09:53:47 ip-172-31-16-98.ec2.internal nrtd[3351]: [TDRV:tdrv_init_one_mla_phase2] Initialized Inferentia...1f.0
Nov 14 09:53:47 ip-172-31-16-98.ec2.internal neuron-rtd[3351]: E1114 09:53:47.511852663    3351 socket_utils_com...65}
Nov 14 09:53:47 ip-172-31-16-98.ec2.internal systemd[1]: Started Neuron Runtime Daemon.
Nov 14 09:53:47 ip-172-31-16-98.ec2.internal nrtd[3351]: [NRTD:RunServer] Server listening on unix:/run/neuron.sock
```

 There may be situations where you start and stop neuron\-rtd\. 

 To start neuron\-rtd, use the following command: 

```
sudo systemctl restart neuron-rtd
```

 To stop neuron\-rtd, use the following command: 

```
sudo systemctl stop neuron-rtd
```

## NeuronCore Groups<a name="tutorial-inferentia-launching-neuroncore-groups"></a>

 NeuronCores \(NC\) are the four execution units inside the Inferentia devices\. Multiple NCs can be combined to form a NeuronCore Group \(NCG\)\. The Neuron framework layer automatically creates a default NeuronCore Group\. To view a list of available NCGs, use the following command: 

```
 neuron-cli list-ncg
```

 Your active NCGs will be displayed in the output as follows: 

```
Device 1 NC count 4
+-------+----------+--------------------+----------------+
| NCG ID| NC COUNT | DEVICE START INDEX | NC START INDEX |
+-------+----------+--------------------+----------------+
|     1 |        1 |                  0 |              0 |
|     2 |        1 |                  0 |              1 |
|     3 |        2 |                  0 |              2 |
+-------+----------+--------------------+----------------+
```

 If you have not configured a NCG, the command will return the following output: 

```
No NCG Found
```

 If you need to unload all models and delete all the NCGs created by the framework, use the following command: 

```
neuron-cli reset
```

## Listing Models<a name="tutorial-inferentia-launching-listing"></a>

 Models can be loaded into a NCG\. Multiple models can be loaded into a single NCG, but only one model can be in the STARTED state\. You can only run inference on models in the STARTED state\. 

 To view all models loaded into the NCG, use the following command:  

```
$ neuron-cli list-model
```

 The models loaded into the NCG will be displayed in the output as follows: 

```
Found 3 model
10003 MODEL_STATUS_LOADED 1
10001 MODEL_STATUS_STARTED 1
10002 MODEL_STATUS_STARTED 1
```

 In the output, 10001 and 10002 are unique identifiers for models loaded in the NCG\. If you have not loaded any models into the NCG, you will see the following output: 

```
Found 0 models
```

## View Resource Usage<a name="tutorial-inferentia-launching-resource-usage"></a>

 Each model loaded into the NCG consumes an amount of memory on the host and device and a percentage of the NeuronCore\. The NGC usage can be viewed by running the following command: 

```
neuron-top
```

 The NGC usage will be displayed in the output as follows: 

```
neuron-top - 2019-11-13 23:57:08
NN Models: 3 total, 2 running
Number of VNCs tracked: 2
0000:00:1f.0 Utilizations: Neuron core0 0.00, Neuron core1 0.00, Neuron core2 0, Neuron core3 0,
DLR Model   Node ID   Subgraph   Exec. Unit       Host Mem   MLA Mem     Neuron core %
10003       1         0          0000:00:1f.0:0   384        135660544   0.00
10001       3         0          0000:00:1f.0:0   384        67633152    0.00
10002       1         0          0000:00:1f.0:1   384        135660544   0.00
```

 If you haven’t loaded any models, your output will look like the following: 

```
NN Models: 0 total, 0 running
Number of VNCs tracked: 0
DLR Model   Node ID   Subgraph   Exec. Unit   Host Mem   MLA Mem   Neuron core %
```

**Next Step**  
[Using the DLAMI with AWS Neuron](tutorial-inferentia-using.md)