
Vagrant.configure("2") do |config|
  
  config.vm.box = "centos/7"
  N = 3
  (1..N).each do |machine_id|
    config.vm.define "machine#{machine_id}" do |machine|
      machine.vm.hostname = "machine#{machine_id}"
      machine.vm.network "private_network", ip: "192.168.56.#{101+machine_id}"
        machine.vm.provider "virtualbox" do |vb|
          vb.memory = 1024
          vb.cpus = 1
        end
    end
  end
 
  config.vm.define "controler" do |controler|
    controler.vm.hostname = "controler"
    controler.vm.network "private_network", ip: "192.168.56.101"
    controler.vm.provider "virtualbox" do |vb|
      vb.memory = 1024
      vb.cpus = 1
    end
    
    (1..N).each do |machine_n|
        controler.vm.provision "file", source: ".vagrant/machines/machine#{machine_n}/virtualbox/private_key", destination: "/home/vagrant/machines/machine#{machine_n}.private_key"
        controler.vm.provision "shell", inline: "chmod 0600 /home/vagrant/machines/machine#{machine_n}.private_key"
    end
    controler.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "playbook.yml"
      ansible.verbose = true
      ansible.install = true
      ansible.limit = "all"
      ansible.inventory_path = "hosts"
    end 
  end
end
