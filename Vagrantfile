#!/usr/bin/env ruby

require 'berkshelf/vagrant'
require 'vagrant-vbguest' unless defined? VagrantVbguest::Config

Vagrant::Config.run do |config|

  images = {
    precise: "http://files.vagrantup.com/precise64.box",
    cent6: 'https://dl.dropbox.com/u/7225008/Vagrant/CentOS-6.3-x86_64-minimal.box'
  }

  images.each do |image, url|
    config.vm.define image do |box|
      box.vm.box = image.to_s
      box.vm.box_url = url

      # box.vm.network :bridged

      box.vm.customize ["modifyvm", :id, "--cpus", 1, "--memory", 512]    
      box.vm.share_folder("v-root", "/vagrant", ".")
      config.vm.share_folder("v-ssh", "/tmp/.ssh", "#{Dir.home}/.ssh")
      config.vbguest.auto_update = true
      box.vm.provision :chef_solo do |chef|
        chef.add_recipe 'vagrant-ssh-config'
        chef.add_recipe 'redis'
        chef.json = {
          "redis" => {
            "install_type" => 'source',
            "source" => {
              "release" => '2.4.18'
            }
          }
        }
      end
    end
  end

end