#!/usr/bin/env ruby

require 'berkshelf/vagrant'
require 'vagrant-vbguest' unless defined? VagrantVbguest::Config

Vagrant::Config.run do |config|

  config.berkshelf.config_path = './knife.rb'

  images = {
    precise: "http://files.vagrantup.com/precise64.box"
  }

  images.each do |key, url|
    config.vm.define key do |box|
      box.vm.box_url = url
      box.vm.customize ["modifyvm", :id, "--cpus", 1, "--memory", 512]    
      box.vm.share_folder("v-root", "/vagrant", ".")
      config.vm.share_folder("v-ssh", "/tmp/.ssh", "#{Dir.home}/.ssh")
      config.vbguest.auto_update = true
      box.vm.provision :chef_solo do |chef|
        chef.add_recipe 'vagrant-ssh-config'
      end
    end
  end

end