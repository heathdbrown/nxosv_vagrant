# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define  "n9kv1" do |node|
    node.vm.box = "nxos/9.3.5"
    node.vm.boot_timeout = 600
    node.ssh.insert_key = false
    node.vm.post_up_message = "N9kv1 up"
    node.vm.box_check_update = false
    # node.vm.provision "shell", inline: "vsh -r /var/tmp/set_vsh_as_default.cmd"

    config.vm.provider "virtualbox" do |vb|
      # vb.memory = "4096"
      # vb.customize ['modifyvm', :id, '--uart1', '0x3F8', 4, '--uartmode1', 'disconnected']
      # vb.customize ['modifyvm', :id, '--uart2', '0x2F8', 3, '--uartmode2', 'disconnected']
      vb.customize ["modifyvm",:id,"--nicpromisc2","allow-all"]
      vb.customize ["modifyvm",:id,"--nicpromisc3","allow-all"]
    end

    #  eth1/1 connected to link2
    node.vm.network :private_network, virtualbox__intnet: "link2", auto_config: false
    node.vm.network :private_network, virtualbox__intnet: "link3", auto_config: false

    node.vm.network :forwarded_port, guest: 22, host: 3122, id: 'ssh' , auto_config: false
    # Forward API Ports
    node.vm.network :forwarded_port, guest: 80, host: 3180, id: 'http', auto_correct: true
    node.vm.network :forwarded_port, guest: 443, host: 3143, id: 'https', auto_correct: true
    node.vm.network :forwarded_port, guest: 830, host: 3130, id: 'netconf', auto_correct: true
  end

  config.vm.define "n9kv2" do |node|
    node.vm.box =  "nxos/9.3.5"
    node.vm.boot_timeout = 600
    node.ssh.insert_key = false
    node.vm.post_up_message = "N9kv2 up"
    node.vm.box_check_update = false
    # node.vm.provision "shell", inline: "vsh -r /var/tmp/set_vsh_as_default.cmd"

    # n9000v defaults to 8G RAM, but only needs 4G
    config.vm.provider "virtualbox" do |vb|
      # Customize the amount of memory on the VM:
      # vb.memory = "4096"
      # Disconnect serial port to avoid conflict
      # vb.customize ['modifyvm', :id, '--uart1', '0x3F8', 4, '--uartmode1', 'disconnected']
      # vb.customize ['modifyvm', :id, '--uart2', '0x2F8', 3, '--uartmode2', 'disconnected']
      vb.customize ["modifyvm",:id,"--nicpromisc2","allow-all"]
      vb.customize ["modifyvm",:id,"--nicpromisc3","allow-all"]
    end

    # eth1/1 connected to link2,
    node.vm.network :private_network, virtualbox__intnet: "link2", auto_config: false
    node.vm.network :private_network, virtualbox__intnet: "link3", auto_config: false

    # Explicity set SSH Port to avoid conflict and for provisioning
    node.vm.network :forwarded_port, guest: 22, host: 3222, id: 'ssh', auto_correct: true

    # Forward API Ports
    node.vm.network :forwarded_port, guest: 80, host: 3280, id: 'http', auto_correct: true
    node.vm.network :forwarded_port, guest: 443, host: 3243, id: 'https', auto_correct: true
    node.vm.network :forwarded_port, guest: 830, host: 3230, id: 'netconf', auto_correct: true
  end

  config.vm.define "ubuntu" do |node|
    node.vm.box = "ubuntu/focal64"
    node.vm.post_up_message = "Ubuntu up"

    config.vm.provider "virtualbox" do |vb|
      # vb.memory = "4096"
      # vb.customize ['modifyvm', :id, '--uart1', '0x3F8', 4, '--uartmode1', 'disconnected']
      # vb.customize ['modifyvm', :id, '--uart2', '0x2F8', 3, '--uartmode2', 'disconnected']
      vb.customize ["modifyvm",:id,"--nicpromisc2","allow-all"]
      vb.customize ["modifyvm",:id,"--nicpromisc3","allow-all"]
    end

    node.vm.network :private_network, virtualbox__intnet: "link3", auto_config: false

    node.vm.network :forwarded_port, guest: 22, host: 3322, id: 'ssh', auto_correct: true
  end
end
