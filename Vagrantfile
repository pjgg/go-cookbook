# -*- mode: ruby -*-
# vi: set ft=ruby :
 
# Adjustable settings
CFG_TZ = "UTC"     # timezone, like US/Pacific, US/Eastern, UTC, Europe/Warsaw, etc.


# Provisioning script
node_script = <<SCRIPT
#!/bin/bash

# set timezone
echo "#{CFG_TZ}" > /etc/timezone    
dpkg-reconfigure -f noninteractive tzdata

# install a few base packages
apt-get update

echo Provisioning is complete
SCRIPT
 
 
# Configure VM server
VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.berkshelf.enabled    = true
  config.omnibus.chef_version = :latest
  config.vm.synced_folder "./share", "/vagrant/share"
  
  config.vm.define :delta do |x|
    x.vm.box = "hashicorp/precise64"
    x.vm.hostname = "microrrelatosGo"
 
    x.vm.provider :aws do |aws, override|
      aws.access_key_id = "XXX"
      aws.secret_access_key = "XXX"
      aws.keypair_name = "microrrelatosContinuosIntegration"
      aws.ami = "ami-5756bb3c"
      aws.region = "us-east-1"
      aws.instance_type = "t2.small"
      aws.security_groups = "microrrelatos-continuos-integration"
 
      override.vm.box = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
      override.ssh.username = "ubuntu"
      override.ssh.private_key_path = "/Users/pablojosegonzalezgranados/Documents/UTAD/proyectoFinMaster/microrrelatosContinuosIntegration.pem"


    x.vm.provision :chef_solo do |chef|
      chef.cookbooks_path = ["cookbooks"]
      chef.log_level = 'info'
      chef.add_recipe :apt
      chef.add_recipe 'git'
      chef.add_recipe 'go'
      chef.add_recipe 'nginx'
      
      chef.json = {
          'go' => {
              'server' => '127.0.0.1',
              'agent' => {
                  'auto_register' => true,
                  'instance_count' => 1
              }
          },
         :git    => {
             :prefix => "/usr/local"
         }
      }

    end
    end

  end

end






