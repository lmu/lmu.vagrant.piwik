# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "canonical/trusty64"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.define "Piwik-DB2" do |piwik_db2|
    piwik_db2.vm.provider "virtualbox" do |vb|
      vb.name = "Piwik-DB2"
      vb.memory = 4096
      vb.cpus = 4
    end
    piwik_db2.vm.network :private_network, ip: "192.168.1.40"
  end

  config.vm.define "Piwik-Master2" do |piwik_master2|
    piwik_master2.vm.provider "virtualbox" do |vb|
      vb.name = "piwik-master2"
      vb.memory = 4096
      vb.cpus = 4
    end
    piwik_master2.vm.network :private_network, ip: "192.168.1.45"
  end

  config.vm.define "Piwik-Worker2" do |piwik_worker2|
    piwik_worker2.vm.provider "virtualbox" do |vb|
      vb.name = "piwik-worker2"
      vb.memory = 2048
      vb.cpus = 2
    end
    piwik_worker2.vm.network :private_network, ip: "192.168.1.46"
  end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL

  # Provisioning only on last maschine but for all:
  config.vm.provision "ansible" do |ansible|
    ansible.groups = {
      "piwik-dbs" => ["piwik_db2"],
      "piwik-workers" => ["piwik_master2", "piwik_worker2"],
      "piwik-masters" => ["piwik_master2"],
    }
    ansible.verbose = ""
    ansible.limit = "all"
    ansible.playbook = "lmu.ansible.playbooks/base-preseed.yml"
  end
end
