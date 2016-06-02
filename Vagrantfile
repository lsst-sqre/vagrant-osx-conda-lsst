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
  # config.ssh.private_key_path = "/Users/jmatt/.ssh/id_rsa"
  # config.ssh.insert_key = true

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "AndrewDryga/vagrant-box-osx" #"http://files.dryga.com/boxes/osx-yosemite-0.2.1.box"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  #config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
    vb.memory = "16384"
    vb.cpus = 8
    file_to_disk = "./data/lsst_conda.vdi"
    unless File.exists?(file_to_disk)
      vb.customize ['createhd',
                    '--filename', file_to_disk,
                    '--format', 'VDI',
                    '--size', 100 * 1024]
    end
    vb.customize ['storageattach', :id,
                  '--storagectl', 'SATA',
                  '--port', 1, '--device', 0,
                  '--type', 'hdd', '--medium', file_to_disk]
  end
  #
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
  config.vm.provision "file", source: "~/.aws/config", destination: "~/.aws/config"
  config.vm.provision "file", source: "~/.aws/credentials", destination: "~/.aws/credentials"
  config.vm.provision "shell", inline: <<-SHELL
    diskutil partitionDisk /dev/disk0 gpt hfsx lsst 0b
    diskutil mount /dev/disk0s2
    rm /usr/local/share/man/man1/brew-cask.1
    brew update
    brew upgrade
    brew install awscli
    ln -s /Volumes/lsst /Users/vagrant/lsst
    git clone https://github.com/jmatt/conda-lsst.git /Users/vagrant/lsst/conda-lsst
    chown -R vagrant:staff /Volumes/lsst
    chown -R vagrant:staff /Users/vagrant/lsst
    chown -R vagrant:admin /usr/local
  SHELL
end
