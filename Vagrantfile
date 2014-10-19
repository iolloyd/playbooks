# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end

  config.vm.define "web" do |app|
    app.vm.host_name = "web.swb.dev"
    app.vm.box = "centos"
    app.vm.network :private_network, ip: "10.0.10.10"
    app.vm.synced_folder "./swb", "/opt/BoxUK/swb-webapp", 
      type: "nfs"
  end

  config.vm.define "db" do |app|
    app.vm.host_name = "db.swb.dev"
    app.vm.box = "centos"
    app.vm.network :private_network, ip: "10.0.10.11"
  end

  config.vm.provision :hosts do |provisioner|
      provisioner.add_host "10.0.10.10", ["db.swb.dev"]
      provisioner.add_host "10.0.10.11", ["web.swb.dev"]

  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "provisioning/main.yml"
    ansible.sudo = true
  end

end
