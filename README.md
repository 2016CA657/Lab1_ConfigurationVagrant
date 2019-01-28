# CA675 Vagrant File

This will provide a convenient baseline for the practicals for CA675

## Step by Step

1. Install [VirtualBox][https://www.vagrantup.com/]
2. Install [Vagrant][https://www.vagrantup.com/]
3. CD into the directory where you want to put your VagrantFile
    * Note for DCU lab machine users - When creating a VM, you should create a folder in d:\virtual (or d:\temp) and store the VM there (or on a personal USB hard drive)
4. `vagrant init ubuntu/trusty64` will create a vagrant box (and your vagrant file) and set up your virtual machine
    * Note you can only have one VagrantFile in any given directory
5. `vagrant up` will start the process of downloading and configuring the VM
6. Use `vagrant ssh` to ssh into the running machine
7. Alternatively, you can then use putty to ssh into `localhost` port `2222` (username/password is `vagrant`)
8. You can then open the `VagrantFile` in a text editor and edit as necessary
    * You might wish to change the number of VCPUs, Memory Allocation
    * You might wish to add or remove shared directories
    * For more details on Vagrantfile commands see https://www.vagrantup.com/docs/vagrantfile/
    * During the course of the labs for this module we will work further on this vagrant file, building upon the blank intial VagrantFile to create one which provisions the VM with multiple cloud technologies.
9. `vagrant halt` shuts down the VM
10. `vagrant destroy` deletes the VM

<!---
As an alternative to Vagrant, you can install the Cloudera QuickStart VM which runs Cloudera Hadoop Distribution
   * Video tutorial: https://www.youtube.com/watch?v=BeCtjd86YXo
   * Installation instruction with VirtualBox: http://www.cse.scu.edu/~mwang2/projects/CDH_installConfig1_13m.pdf 
--->

<!---
The QuickStart VM is fully functional and you can test many Hadoop services, even though it is running as a single-node cluster.
--->
