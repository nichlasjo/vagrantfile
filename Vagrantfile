$script = <<-SCRIPT
printf "172.17.177.21 node1\n172.17.177.22 node2\n172.17.177.23 node3\n" >> /etc/hosts
SCRIPT

Vagrant.configure(2) do |config|
    config.vm.box = "centos/7"
    config.ssh.forward_agent = true
    config.ssh.insert_key = false

    config.vm.define "node1" do |config|
    config.vm.network "private_network", ip: "172.17.177.21"
    ENV['ANSIBLE_INVENTORY'] = "inventory"
    config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
  end

    config.vm.define "node2" do |config|
    config.vm.network "private_network", ip: "172.17.177.22"
    ENV['ANSIBLE_INVENTORY'] = "inventory"
    config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
  end

    config.vm.define "node3" do |config|
    config.vm.network "private_network", ip: "172.17.177.23"
    ENV['ANSIBLE_INVENTORY'] = "inventory"
    config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
  end

  config.vm.define 'controller' do |config|
    config.vm.network "private_network", ip: "172.17.177.11"
    ENV['ANSIBLE_INVENTORY'] = "inventory"
    config.vm.synced_folder ".", "/vagrant", type: "virtualbox"
    config.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: ".ssh/id_rsa"
    config.vm.provision "shell", inline: $script
      config.vm.provision :ansible_local do |ansible|
      ansible.playbook = "vagrant-local.yml"
      ansible.install = true
      ansible.verbose = true
      ansible.limit = 'all'
    end
  end
end
