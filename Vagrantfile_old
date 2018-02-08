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
    #
    config.vm.network "forwarded_port", guest: 3000, host: 3000
    config.vm.network "forwarded_port", guest: 5000, host: 5000
    config.vm.network "forwarded_port", guest: 8000, host: 8000
    config.vm.network "forwarded_port", guest: 8888, host: 8888
    config.vm.network "forwarded_port", guest: 50700, host: 50700

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

    apt-get update
    apt-get dist-upgrade -y

    apt-get install -y -q git wget curl build-essential python python-dev python-setuptools
    apt-get install -y -q gfortran libopenblas-dev liblapack-dev
    apt-get install -y -q libncurses5-dev pkg-config libfreetype6-dev libpng12-dev

    easy_install pip
    pip install -U pip setuptools cython
    pip install -U virtualenv virtualenvwrapper

    apt-get -y -q install software-properties-common htop maven
    add-apt-repository ppa:webupd8team/java
    apt-get -y -q update
    echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections
    apt-get -y -q install oracle-java8-installer > /dev/null 2>&1
    update-java-alternatives -s java-8-oracle

    apt-get autoremove -q -y

    ssh-keygen -t rsa -N "" -f /root/.ssh/id_rsa > /dev/null 2>&1
    cp /root/.ssh/id_rsa.pub /root/.ssh/authorized_keys
    mkdir -p /var/run/sshd
    echo "Host localhost\n    UserKnownHostsFile=/dev/null\n    StrictHostKeyChecking=no" >> /root/.ssh/config

    echo "hadoop downloading and installing"
    wget --quiet http://ftp.heanet.ie/mirrors/www.apache.org/dist/hadoop/common/stable/hadoop-2.7.2.tar.gz
    tar xzf hadoop-2.7.2.tar.gz
    mv hadoop-2.7.2 /usr/local/hadoop
    chmod -R 777 /usr/local/hadoop

    echo "\n\n export JAVA_HOME=/usr/lib/jvm/java-8-oracle/jre/" >> /usr/local/hadoop/etc/hadoop/hadoop-env.sh
    
    echo "hive downloading and installing"
    wget --quiet http://ftp.heanet.ie/mirrors/www.apache.org/dist/hive/hive-2.0.0/apache-hive-2.0.0-bin.tar.gz
    tar xzf apache-hive-2.0.0-bin.tar.gz
    mv apache-hive-2.0.0-bin /usr/local/hive
    chmod -R 777 /usr/local/hive
    
    echo "pig downloading and installing"
    wget --quiet http://ftp.heanet.ie/mirrors/www.apache.org/dist/pig/latest/pig-0.15.0.tar.gz
    tar xzf pig-0.15.0.tar.gz
    mv pig-0.15.0 /usr/local/pig
    chmod -R 777 /usr/local/pig

    mkdir -p /user/hive

    # set the hive warehouse up
    /usr/local/hadoop/bin/hadoop fs -mkdir /user/hive/warehouse
    chmod -R 777 /user/hive 

    echo "brewing up a storm"
    wget --quiet http://mirrors.whoishostingthis.com/apache/storm/apache-storm-0.10.0/apache-storm-0.10.0.tar.gz
    tar zxf apache-storm-0.10.0.tar.gz
    mv apache-storm-0.10.0 /usr/local/storm
    chmod -R 777 /usr/local/storm
    
    SHELL

    # make user-mode configuration changes
    config.vm.provision "shell", privileged: false, inline: <<-SHELL
    cd /home/vagrant
    echo 'export HIVE_HOME=/usr/local/hive' >> ~/.bashrc
    echo "export HADOOP_HOME=/usr/local/hadoop" >> ~/.bashrc
    echo 'export PATH=$PATH:/usr/local/hadoop/bin' >> ~/.bashrc
    echo 'export PATH=$PATH:/usr/local/hadoop/sbin' >> ~/.bashrc

    mkdir -p ~/.virtualenvs
    echo "\n\n export WORKON_HOME=/home/vagrant/.virtualenvs" >> ~/.bashrc
    echo "\n\n source /usr/local/bin/virtualenvwrapper.sh" >> ~/.bashrc
    echo "\n\n alias ipython='jupyter notebook --no-browser --ip=0.0.0.0 --port=8888'" >> ~/.bashrc

    echo "\n\n export JAVA_HOME=/usr/lib/jvm/java-8-oracle/jre/" >> ~/.bashrc

    source ~/.bashrc

    # keep a log of what happened in the vagrant
    /usr/local/hive/bin/schematool -initSchema -dbType derby

    echo "Sparking up. This takes a very long time..."
    wget --quiet http://ftp.heanet.ie/mirrors/www.apache.org/dist/spark/spark-1.6.1/spark-1.6.1.tgz 
    tar zxf spark-1.6.1.tgz
    cd spark-1.6.1
    build/mvn -Pyarn -Phadoop-2.6 -Dhadoop.version=2.7.0 -DskipTests clean package
    cd ..
    echo "Told you"
    
    SHELL
end
