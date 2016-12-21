# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# Special Settings for this Vagrant Environment
USE_PUBLIC_NETWORK = false
#USE_PUBLIC_NETWORK = true
PRIVATE_NETWORK_BASE = "192.168.1"
#PRIVATE_NETWORK_BASE = PRIVATE_NETWORK_BASE + ""
PUBLIC_NETWORK_BASE = "137.193.211"
#PUBLIC_NETWORK_BASE = "192.168.2"
AUTOSTART_CLUSTER = false
#AUTOSTART_CLUSTER = true

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Every Vagrant virtual environment requires a box to build off of.
  #config.vm.box = "ubuntu/trusty64"
  #config.vm.box = "ubuntu/xenial64"
  config.vm.box = "centos/7"

  config.ssh.forward_agent = true
  #config.ssh.private_key_path = "~/.ssh/id_rsa"

  config.vm.define "piwik-db.zuv.uni-muenchen.de", autostart: AUTOSTART_CLUSTER do |node|
    node.vm.provider "virtualbox" do |vb|
      vb.name = "Piwik-DB"
      vb.memory = 4096
      vb.cpus = 4
      vb.customize ["modifyvm", :id,
                    "--cpuexecutioncap", "50",
                    "--groups", "/Vagrant/LMU/Piwik"
                   ]
    end
    node.vm.network :private_network, ip: PRIVATE_NETWORK_BASE + ".199"
    if USE_PUBLIC_NETWORK
      node.vm.network :public_network, ip: PUBLIC_NETWORK_BASE + ".199"
    end
  end

  config.vm.define "piwik-master.zuv.uni-muenchen.de", autostart: AUTOSTART_CLUSTER do |node|
    node.vm.provider "virtualbox" do |vb|
      vb.name = "Piwik-Master"
      vb.memory = 4096
      vb.cpus = 4
      vb.customize ["modifyvm", :id,
                    "--cpuexecutioncap", "50",
                    "--groups", "/Vagrant/LMU/Piwik"
                   ]
    end
    node.vm.network :private_network, ip: PRIVATE_NETWORK_BASE + ".195"
    if USE_PUBLIC_NETWORK
      node.vm.network :public_network, ip: PUBLIC_NETWORK_BASE + ".195"
    end
  end

  config.vm.define "piwik-worker.zuv.uni-muenchen.de", autostart: AUTOSTART_CLUSTER do |node|
    node.vm.provider "virtualbox" do |vb|
      vb.name = "Piwik-Worker"
      vb.memory = 2048
      vb.cpus = 2
      vb.customize ["modifyvm", :id,
                    "--cpuexecutioncap", "50",
                    "--groups", "/Vagrant/LMU/Piwik"
                   ]
    end
    node.vm.network :private_network, ip: PRIVATE_NETWORK_BASE + ".196"
    if USE_PUBLIC_NETWORK
      node.vm.network :public_network, ip: PUBLIC_NETWORK_BASE + ".196"
    end
  end

  config.vm.define "special.zuv.uni-muenchen.de", autostart: AUTOSTART_CLUSTER do |node|
    node.vm.provider "virtualbox" do |vb|
      vb.name = "Special"
      vb.memory = 4096
      vb.cpus = 4
      vb.customize ["modifyvm", :id,
                    "--cpuexecutioncap", "50",
                    "--groups", "/Vagrant/LMU"
                   ]
    end
    node.vm.network :private_network, ip: "192.168.1.5"
    node.vm.network :private_network, ip: PRIVATE_NETWORK_BASE + ".200"
    if USE_PUBLIC_NETWORK
      node.vm.network :public_network, ip: PUBLIC_NETWORK_BASE + ".200"
    end
  end


  config.vm.define "piwiktest.zuv.uni-muenchen.de", primary: true do |node|
    node.vm.provider "virtualbox" do |vb|
      vb.name = "Piwik-Test"
      vb.memory = 8192
      vb.cpus = 8
      vb.customize ["modifyvm", :id,
                    "--cpuexecutioncap", "50",
                    "--groups", "/Vagrant/LMU/Piwik"
                   ]
    end
    node.vm.network :private_network, ip: PRIVATE_NETWORK_BASE + ".85"
    if USE_PUBLIC_NETWORK
      node.vm.network :public_network, ip: PUBLIC_NETWORK_BASE + ".85"
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "lmu.ansible.playbooks/base-preseed.yml"
    #ansible.verbose = "vvv"
  end

  # config.vm.provision "ansible" do |ansible|
  #   ansible.playbook = "lmu.ansible.playbooks/base-preseed.yml"
  #   #ansible.playbook = "lmu.ansible.playbooks/piwik.yml"
  #   ansible.groups = {
  #     "piwik-production-dbs" => ["piwik-db.zuv.uni-muenchen.de"],
  #     "piwik-production-workers"  => ["piwik-master.zuv.uni-muenchen.de",
  #                                     "piwik-worker.zuv.uni-muenchen.de"],
  #     "piwik-production-masters"  => ["piwik-master.zuv.uni-muenchen.de"],
  #     "piwik-production-frontends" => ["special.zuv.uni-muenchen.de"],
  #     "piwik-test-dbs" => ["piwiktest.zuv.uni-muenchen.de"],
  #     "piwik-test-workers"  => ["piwiktest.zuv.uni-muenchen.de"],
  #     "piwik-test-masters"  => ["piwiktest.zuv.uni-muenchen.de"],
  #     "piwik-test-frontends" => ["piwiktest.zuv.uni-muenchen.de"],
  #     "piwik-dbs:children" => ["piwik-production-dbs", "piwik-test-dbs"],
  #     "piwik-workers:children" => ["piwik-production-workers", "piwik-test-workers"],
  #     "piwik-masters:children" => ["piwik-production-masters", "piwik-test-masters"],
  #     "piwik-frontends:children" => ["piwik-production-frontends", "piwik-test-frontends"],
  #     "piwik:children" => ["piwik-dbs", "piwik-masters", "piwik-workers"],
  #   }
  #   #ansible.verbose = "vvvv"
  #   #ansible.verbose = "vvv"
  #   #ansible.verbose = "vv"
  #   #ansible.verbose = "v"
  #   ansible.verbose = ""
  #   ansible.limit = "all"
  #   #ansible.tags = ["setup", "configuration", "update"]
  #   #ansible.skip_tags = ["update"]
  #   #ansible.start_at_task = "Install Piwik"
  #   #ansible.ask_vault_pass = true
  # end
end
