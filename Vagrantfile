# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANTFILE_API_VERSION = "2"

lab = {
  "control"  => { :dns => "control.example.com",  :ip => "192.168.4.200", :script => "control.sh" },
  "ansible1" => { :dns => "ansible1.example.com", :ip => "192.168.4.201", :script => "managed.sh" },
  "ansible2" => { :dns => "ansible2.example.com", :ip => "192.168.4.202", :script => "managed.sh" },
  "ansible3" => { :dns => "ansible2.example.com", :ip => "192.168.4.203", :script => "managed.sh" },
}

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  check_guest_additions = false
  config.ssh.insert_key = false
  config.ssh.forward_agent= true
  config.ssh.forward_x11 = false
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.box = "bento/ubuntu-24.04"
#  config.vm.box = "bento/opensuse-leap-15"

#  config.vm.provider :libvirt do |node|
#    node.memory = 512
#    node.cpus = 1
#  end # end provider

  config.vm.provider :vmware_fusion do |node|
    node.vmx["ethernet0.virtualDev"] = "vmxnet3"
    node.vmx["memsize"] = 512
    node.vmx["numvcpus"] = 1
  end # end provider

  lab.each_with_index do |(lab_node, info), index|
    config.vm.define lab_node do |cfg|
      cfg.vm.network :private_network, ip: "#{info[:ip]}"
      #cfg.vm.network :private_network, ip: "#{info[:ip]}", :mode => 'bridge'
      cfg.vm.provision "#{lab_node}", type: "shell", path: "#{info[:script]}"
      cfg.vm.hostname = "#{info[:dns]}"
    end # end config
  end # end lab
end
