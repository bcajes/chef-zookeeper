#!/usr/bin/env ruby

box = ENV['VAGRANT_BOX'] || 'opscode_ubuntu-12.04_provisionerless'
Vagrant.configure('2') do |config|
  config.omnibus.chef_version = :latest
  config.berkshelf.enabled = true
  config.vm.hostname = 'zookeeper'
  config.vm.box = box
  config.vm.box_url = "https://opscode-vm.s3.amazonaws.com/vagrant/#{box}.box"
  config.omnibus.chef_version = :latest
  ['192.168.50.5', '192.168.50.4', '192.168.50.3'].each do |zknode|
    config.vm.define zknode do |zookeeper|
      zookeeper.vm.network :private_network, ip: zknode
      zookeeper.vm.provision :shell do |shell|
        shell.inline = 'test -f $1 || (sudo apt-get update -y && touch $1)'
        shell.args = '/var/run/apt-get-update'
      end

      zookeeper.vm.provision :chef_solo do |chef|
        chef.run_list = [
                         'recipe[zookeeper::default]'
                        ]
      end
    end
  end
end
