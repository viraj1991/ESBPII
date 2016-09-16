#ESBPII Assingment#
#How to do vMotion Migration in vSphere client#

###Name - P.M.V.M.Gunasena###
###IT Number - IT13049546###

*********
# Outline#
1. Introduction
2. Virtual machine requirements for vMotion Migration
3. Hosts requirements for vMotion Migration
4. Identify CPU Characteristics
5. Compatibility requirements
6. vMotion Migration steps

********
#Introduction#
Vmotion VMware is a technology that enables the migration of virtual machines from one ESXi host to another host without losing service.

![](http://i.imgur.com/OLuh4oZ.png)

A vMotion migration moves a powered-on virtual machine from one host to another.
vMotion can be used to:

- Improve overall hardware utilization.
- Allow continued virtual machine operation while accomadating scheduled hardware downtime.
- Allow vSphere Distributed Resource Scheduler (DRS) to balance virtual machines across hosts.

![](http://i.imgur.com/GgtYdkc.png)

#Virtual machine requirements for vMotion Migration#

- A virtual machine must not thave a connection to a virtual device (such as a CD-ROM or floppy drive) with a local image mounted.
- A virtual machine must not have a connection to an internal vSwitch(vSwitch with zero uplink adapters).
- A virtual machine must not have CPU affinity configured.

#Hosts requirements for vMotion Migration#

- Visibility to all storage (Fibre Channel, iSCSI, or NAS)used by the virtual machine.
- At least a Gigabyte Ethernet network:
	- Four concurrent vMotion migrations on a 1Gbps network
	- Eight concurrent vMotion migrations on a 10Gbps network
- Access to the same physical networks.
- Compatible CPUs.
- To do vMotion Migration in one physical machine it need at least 12GB of RAM to run 2 seperate VMware EXSi baremetal hypervisors.

#Identify CPU characteristics#

One way to determine whether or not you have compatible CPUs is to use the vmware CPU identification utility which is downloadable from vmware. You can download that and so burn it onto a CD, pop that CD into each host one at a time and boot the host and you'll see a report as in following picture.

![](http://i.imgur.com/8azvx1I.png)

#Compatibility requirements#

There are several methods which can be used to address vMotion CPU compatiblity requirements:

- Procure servers with identical CPUs.
- Compatibility masking in the vSphere client.
- Enhanced vMotion Compatibility (EVC)
	- Automatically masks off CPU incompatibility
	- A feature of DRS clusters

#vMotion Migration steps#

Actions to be taken through vSphere Client connected to VirtualCenter.

We connect to Virtual Center and gain access to one of the servers 2.

We select the tab Configuration-> Network Adapters and we see that we have visibility of the new connections.

![](http://i.imgur.com/CVFreDQ.png)

Now we look at the tab Configuration-> Networking

![](http://i.imgur.com/Sj72oiv.png)

Click on Add Networking to create the vSwitch.

![](http://i.imgur.com/RPahDUp.png)

Select VMKernel and click on Next.

![](http://i.imgur.com/fJhPiT5.png)

Making a network card or cards that have connected from one server to another (in our case vmnic9) And click on Next.

![](http://i.imgur.com/EFZnrfZ.png)

We set Use this port group for vMotion.

We wrote a Label Network different if you want (optional) and click on Next. We for example we put Vmotion.

![](http://i.imgur.com/kBaVzf4.png)

We set Use the following IP settings:

- IP Address: 50.50.50.1
- Subnet Mask: 255.255.255.252

Click on Next.

![](http://i.imgur.com/28v5cTz.png)

Click on Finish.

![](http://i.imgur.com/an8v0uq.png)

We found that they have created a new virtual switch with Vmotion.

We connect to another server involved.

We select the tab Configuration-> Network Adapters and we see that we have visibility of the new connections.

![](http://i.imgur.com/GS2cEoR.png)

Now we look at the tab Configuration-> Networking

![](http://i.imgur.com/ua4lchq.png)

Click on Add Networking to create the vSwitch.

![](http://i.imgur.com/O3NMVko.png)

Select VMKernel and click on Next.

![](http://i.imgur.com/5fE55IV.png)

Making a network card or cards that have connected from one server to another (in our case vmnic9) And click on Next.

![](http://i.imgur.com/lAtLFBM.png)

We set Use this port group for VMotion.

We wrote a Label Network different if you want (optional) and click on Next. We for example we put Vmotion.

![](http://i.imgur.com/cvEDKmC.png)

We set Use the following IP settings:

- IP Address: 50.50.50.2 (This ip must be different from the server that we configured earlier 1).
- Subnet Mask: 255.255.255.252 (Since we will use only 2 ip's).

Click on Next.

![](http://i.imgur.com/zNtkPHz.png)

Click on Finish.

And now what we will do to ensure that the entire system is working properly migrate a VM from one ESXi to the other using Vmotion functionality you just configured.

We press the right mouse button on a virtual machine.

![](http://i.imgur.com/7qTdkaJ.png)

Click on Migrate.

![](http://i.imgur.com/jVn91H5.png)

Click on Next.

![](http://i.imgur.com/B9qrhAs.png)

Select the target server where we will move the virtual machine.

Click on Next.

![](http://i.imgur.com/GT8skf0.png)

Click on Next.

![](http://i.imgur.com/8HGdTSP.png)

Click on Finish to start the migration.

![](http://i.imgur.com/sS69GH9.png)

Perfect the system has been migrated from an ESXi host to another without losing the service and in just 47 seconds, if we set up another network card this time has reduced considerably, as we have said before Vmotion is able to use multiple cards network for migration.