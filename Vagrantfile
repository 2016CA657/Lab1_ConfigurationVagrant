# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/wily64"

  config.ssh.forward_x11 = true
  config.ssh.forward_agent = true

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  config.vm.network "forwarded_port", guest: 3000, host: 3000
  config.vm.network "forwarded_port", guest: 5000, host: 5000
  config.vm.network "forwarded_port", guest: 8000, host: 8000
  config.vm.network "forwarded_port", guest: 8888, host: 8888

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  dropbox_dir = File.join ENV['HOME'], '/Dropbox'
  drive_dir   = File.join ENV['HOME'], '/Google Drive'

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # Need absolute paths, so the answer here is to leverage the environment variable
  config.vm.synced_folder dropbox_dir, '/home/vagrant/Dropbox'     if File.exists?(dropbox_dir)
  config.vm.synced_folder drive_dir,   '/home/vagrant/GoogleDrive' if File.exists?(drive_dir)

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # vb.gui = true

    # Customize the amount of memory on the VM:
    vb.memory = "8192"
    # set the number of cpus
    vb.cpus = "2"
    # Enable usb for opencv (requires "Oracle VM VirtualBox Extension Pack" for VB users)
    vb.customize ["modifyvm", :id, "--usb", "on"]
    vb.customize ["modifyvm", :id, "--usbehci", "on"]
  end

  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL
  config.vm.provision "shell", inline: <<-SHELL
    set -e

    apt-get update
    apt-get dist-upgrade -y

    apt-get install -y -q git wget curl pandoc build-essential vim-gnome python python-dev python-setuptools
    apt-get install -y -q gfortran libopenblas-dev liblapack-dev
    apt-get install -y -q libncurses5-dev pkg-config libfreetype6-dev libpng12-dev

    easy_install pip
    pip install -U pip setuptools cython
    pip install -U virtualenv virtualenvwrapper

    apt-get -y -q install software-properties-common htop
    add-apt-repository ppa:webupd8team/java
    apt-get -y -q update
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections
    apt-get -y -q install oracle-java8-installer
    update-java-alternatives -s java-8-oracle

    apt-get autoremove -q -y
  SHELL

  # make user-mode configuration changes
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
    mkdir -p ~/.virtualenvs
    echo "\n\n export WORKON_HOME=/home/vagrant/.virtualenvs" >> ~/.bashrc
    echo "\n\n source /usr/local/bin/virtualenvwrapper.sh" >> ~/.bashrc
    echo "\n\n alias ipython='ipython notebook --no-browser --ip=0.0.0.0 --port=8888'" >> ~/.bashrc

    echo "\n\n export JAVA_HOME=/usr/lib/jvm/java-8-oracle/jre/" >> ~/.bashrc
  SHELL
end
