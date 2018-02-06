#CA675 Vagrant File

This will provide a convenient baseline for the practicals for CA675

## Step by Step

1. Install VirtualBox or VMware Player
    * https://www.virtualbox.org/wiki/Downloads
2. Install Vagrant: <https://www.vagrantup.com/>
    * On Windows, it's also useful to install
    * Putty <http://www.putty.org/> to SSH into the local box
    * XMing <http://sourceforge.net/projects/xming/>
    * Configuration Guide <http://www.geo.mtu.edu/geoschem/docs/putty_install.html>
    * Git Powershell <https://help.github.com/articles/set-up-git/>
3. Clone (or Fork) this repository and edit the `VagrantFile` as necessary
    * You might wish to change the number of VCPUs, Memory Allocation
    * You might wish to add or remove shared directories
4. CD into the directory containing the Vagrantfile
5. `vagrant up` will start the process of downloading and configuring the
   virtual machine
6. You can then use putty to ssh into `localhost` port `2222`
   (username/password is `vagrant`)
7. Alternatively, use `vagrant ssh` for normal access
