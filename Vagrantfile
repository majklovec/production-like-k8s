IMAGE_NAME = "bento/ubuntu-16.04"

MASTERS = 1
NODES = 3

Vagrant.configure("2") do |config|
  config.ssh.insert_key = false

  config.vm.provider "virtualbox" do |v|
    v.memory = 1024
    v.cpus = 2
  end

  (1..MASTERS).each do |i|
    config.vm.define "master-#{i}" do |master|
      master.vm.box = IMAGE_NAME
      master.vm.network "private_network", ip: "192.168.50.10"
      master.vm.hostname = "master-#{i}"
      master.vm.provider :virtualbox do |vb|
        vb.name = "master-#{i}"
      end
      master.vm.provision "ansible" do |ansible|
        ansible.playbook = "kubernetes-setup/master-playbook.yml"
        ansible.compatibility_mode = "2.0"
        ansible.verbose = true
        ansible.extra_vars = { keepalived_priority: 90 }
      end
    end
  end

  (1..NODES).each do |i|
    config.vm.define "node-#{i}" do |node|
      node.vm.box = IMAGE_NAME
      node.vm.network "private_network", ip: "192.168.50.#{i + 10}"
      node.vm.hostname = "node-#{i}"
      node.vm.provider :virtualbox do |vb|
        vb.name = "node-#{i}"
      end

      node.vm.provision :ansible do |ansible|
        ansible.playbook = "kubernetes-setup/node-playbook.yml"
        ansible.verbose = true
        ansible.compatibility_mode = "2.0"
        ansible.extra_vars = { keepalived_priority: 100 + i }
      end
    end
  end
end
