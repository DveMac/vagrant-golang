# -*- mode: ruby -*-
# vi: set ft=ruby :

# Adjustable settings
CFG_TZ = "UTC"     # timezone, like US/Pacific, US/Eastern, UTC, Europe/Warsaw, etc.
CFG_IP = "10.254.254.200"   # private IP address

# Provisioning script
node_script = <<SCRIPT
#!/bin/bash

# set timezone
echo "#{CFG_TZ}" > /etc/timezone    

echo "Vagrant provisioning complete"
SCRIPT

# Configure VM server
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.network :private_network, ip: CFG_IP
  config.vm.hostname = "service"
  config.vm.box = "debian-607-x64-vbox4210"
  config.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/debian-607-x64-vbox4210.box"
  config.vm.synced_folder  (ENV['GOPATH'] + "/src"), "/vagrant/src"
  config.vm.provision :puppet do |puppet|
    puppet.module_path      = "modules"
    puppet.manifests_path	= "manifests"
    puppet.manifest_file    = "init.pp"
  end
  config.vm.provision :shell, :inline => node_script
end
