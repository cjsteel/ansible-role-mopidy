# -*- mode: ruby -*-
  # vi: set ft=ruby ts=2 sw=2 tw=0 et :

  role = File.basename(File.expand_path(File.dirname(__FILE__)))
  role_short_name = "mopidy"

  boxes = [
  #  {
  #    :name => "ubuntu-1204",
  #    :box => "bento/ubuntu-12.04",
  #    :ip => '10.0.0.11',
  #    :cpu => "50",
  #    :ram => "256"
  #  },
  #  {
  #    :name => "ubuntu-1404",
  #    :box => "bento/ubuntu-14.04",
  #    :ip => '10.0.0.12',
  #    :cpu => "50",
  #    :ram => "256"
  #  },
    {
      :name => "ubuntu-1604",
#      :box => "ubuntu/xenial64",
      :box => "xenial64dt",
#      :version => "20171221.0.0",
      :ip => '10.0.0.13',
      :cpu => "50",
      :ram => "512"
    },
  #  {
  #    :name => "debian-711",
  #    :box => "bento/debian-7.11",
  #    :ip => '10.0.0.14',
  #    :cpu => "50",
  #    :ram => "256"
  #  },
  #  {
  #    :name => "debian-86",
  #    :box => "bento/debian-8.6",
  #    :ip => '10.0.0.15',
  #    :cpu => "50",
  #    :ram => "256"
  #  },
#    {
#      :name => "centos7",
#      :box => "geerlingguy/centos7",
#      :ip => '10.0.0.16',
#      :cpu => "50",
#      :ram => "256"
#    },
  ]

  Vagrant.configure("2") do |config|
    boxes.each do |box|
# set name vagrant outputs on the console
      config.vm.define box[:name] do |vms|
        vms.vm.box = box[:box]
#        vms.vm.box_version = box[:version]
        #Now we set the hostname "in" the VM
        vms.vm.hostname = "ansible-#{role}-#{box[:name]}"
#        vms.vm.synced_folder ".vagrant/synced", "/home/vagrant"
        vms.vm.provider "virtualbox" do |vbox|
          #Now we set the VirtualBox GUI Name define using vbox.name
          vbox.name = "#{role_short_name}-#{box[:name]}"
          vbox.customize ["modifyvm", :id, "--cpuexecutioncap", box[:cpu]]
          vbox.customize ["modifyvm", :id, "--memory", box[:ram]]
        end
        vms.vm.network :private_network, ip: box[:ip]
        vms.vm.provision "shell",
          inline: "echo 'We have lift off'"
        vms.vm.provision :ansible do |ansible|
          ansible.playbook = "tests/vagrant.yml"
          ansible.verbose = "vv"
        end
      end
    end
  end
