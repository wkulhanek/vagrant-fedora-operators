# -*- mode: ruby -*-
# vi: set ft=ruby :

# Fedora 28 Vagrant box:
box = "generic/fedora28"

# set to taste
hostname = "fedoravm"
ip = "192.168.10.10"

# optional - set to taste
local_folder = "~/Development"
remote_mount = "/home/vagrant/development"

# set with one or more keys
ssh_keys = ["~/.vagrant.d/insecure_private_key", "~/.ssh/vagrant.pem" ]

memory = 4096

Vagrant.configure("2") do |config|

  required_plugins = %w( vagrant-vbguest vagrant-disksize )
  _retry = false
  required_plugins.each do |plugin|
     unless Vagrant.has_plugin? plugin
       system "vagrant plugin install #{plugin}"
      _retry=true
    end
  end

  if (_retry)
    exec "vagrant " + ARGV.join(' ')
  end

  config.vm.box = box
  config.disksize.size = "32GB"

  # setup networking and hostname - works best with vagrant hostsupdater plugin
  config.vm.hostname = "%s.v" % hostname # domain postfix optional good for ssh "*.v" etc
  config.vm.network "private_network", ip: ip

  # setup any shared folders, set in global vars
  config.vm.synced_folder local_folder, remote_mount

  # Disable default /vagrant share
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # setup virtualbox settings
  config.vm.provider :virtualbox do |vb|
    vb.name = hostname
    vb.memory = memory
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "configure_box.yml"
  end
end
