# -*- mode: ruby -*-
# vi: set ft=ruby :

env = ENV.has_key?('APP_ENV') ? ENV['APP_ENV'] : "dev"

def Kernel.is_windows?
    processor, platform, *rest = RUBY_PLATFORM.split("-")
    platform == 'mingw32'
end

Vagrant.configure("2") do |config|
  config.vm.box = "precise64"
  config.vm.hostname = "myapphost"
  config.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/ubuntu-server-12042-x64-vbox4210.box"
  config.vm.network :private_network, ip: "199.199.199.51"
  config.vm.network "public_network"

  if Kernel.is_windows?
    config.vm.synced_folder "./", "/var/www/myapphost/current", :owner => "www-data", :group => "www-data", :nfs => false
  else
    config.vm.synced_folder "./", "/var/www/myapphost/current", :nfs => true
  end

  config.vm.provider "virtualbox" do |v|
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "100"]
    v.customize ["modifyvm", :id, "--memory", 2048]
    v.customize ["modifyvm", :id, "--cpus", 2]
  end

  config.ssh.forward_agent = true

  # Ansible
  config.vm.provision "ansible" do |ansible|
    ansible.inventory_path = "provisioning/hosts/development"
    ansible.sudo = true
    ansible.playbook = "provisioning/site.yml"
  end
end
