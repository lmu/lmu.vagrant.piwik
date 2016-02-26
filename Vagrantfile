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
  config.vm.box = "canonical/trusty64"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.define "piwik-db2.verwaltung.uni-muenchen.de", autostart: AUTOSTART_CLUSTER do |maschine|
    maschine.vm.provider "virtualbox" do |vb|
      vb.name = "Piwik-DB2"
      vb.memory = 4096
      vb.cpus = 4
      vb.customize ["modifyvm", :id,
                    "--cpuexecutioncap", "50",
                    "--groups", "/Vagrant/LMU/Piwik"
                   ]
    end
    maschine.vm.network :private_network, ip: PRIVATE_NETWORK_BASE + ".199"
    if USE_PUBLIC_NETWORK
      maschine.vm.network :public_network, ip: PUBLIC_NETWORK_BASE + ".199"
    end
  end

  config.vm.define "piwik-master2.verwaltung.uni-muenchen.de", autostart: AUTOSTART_CLUSTER do |maschine|
    maschine.vm.provider "virtualbox" do |vb|
      vb.name = "Piwik-Master2"
      vb.memory = 4096
      vb.cpus = 4
      vb.customize ["modifyvm", :id,
                          "--cpuexecutioncap", "50",
                          "--groups", "/Vagrant/LMU/Piwik"
                         ]
    end
    maschine.vm.network :private_network, ip: PRIVATE_NETWORK_BASE + ".195"
    if USE_PUBLIC_NETWORK
      maschine.vm.network :public_network, ip: PUBLIC_NETWORK_BASE + ".195"
    end
  end

  config.vm.define "piwik-worker2.verwaltung.uni-muenchen.de", autostart: AUTOSTART_CLUSTER do |maschine|
    maschine.vm.provider "virtualbox" do |vb|
      vb.name = "Piwik-Worker2"
      vb.memory = 2048
      vb.cpus = 2
      vb.customize ["modifyvm", :id,
                    "--cpuexecutioncap", "50",
                    "--groups", "/Vagrant/LMU/Piwik"
                   ]
    end
    maschine.vm.network :private_network, ip: PRIVATE_NETWORK_BASE + ".196"
    if USE_PUBLIC_NETWORK
      maschine.vm.network :public_network, ip: PUBLIC_NETWORK_BASE + ".196"
    end
  end

  config.vm.define "special.verwaltung.uni-muenchen.de", autostart: AUTOSTART_CLUSTER do |maschine|
    maschine.vm.provider "virtualbox" do |vb|
      vb.name = "Special"
      vb.memory = 4096
      vb.cpus = 4
      vb.customize ["modifyvm", :id,
                    "--cpuexecutioncap", "50",
                    "--groups", "/Vagrant/LMU"
                   ]
    end
    maschine.vm.network :private_network, ip: "192.168.1.5"
    maschine.vm.network :private_network, ip: PRIVATE_NETWORK_BASE + ".200"
    if USE_PUBLIC_NETWORK
      maschine.vm.network :public_network, ip: PUBLIC_NETWORK_BASE + ".200"
    end
  end


  config.vm.define "piwiktest2.verwaltung.uni-muenchen.de", primary: true do |maschine|
    maschine.vm.provider "virtualbox" do |vb|
      vb.name = "Piwiktest2"
      vb.memory = 8192
      vb.cpus = 8
      vb.customize ["modifyvm", :id,
                    "--cpuexecutioncap", "50",
                    "--groups", "/Vagrant/LMU/Piwik"
                   ]
    end
    maschine.vm.network :private_network, ip: PRIVATE_NETWORK_BASE + ".85"
    if USE_PUBLIC_NETWORK
      maschine.vm.network :public_network, ip: PUBLIC_NETWORK_BASE + ".85"
    end
  end


  if ENV['OS'] != "Windows_NT" # Windows don't support ansible,
    # Provisioning only on last maschine but for all:
    config.vm.provision "ansible" do |ansible|
      ansible.groups = {
        "piwik-production-dbs" => ["piwik-db2.verwaltung.uni-muenchen.de"],
        "piwik-production-workers"  => ["piwik-master2.verwaltung.uni-muenchen.de",
                                        "piwik-worker2.verwaltung.uni-muenchen.de"],
        "piwik-production-masters"  => ["piwik-master2.verwaltung.uni-muenchen.de"],
        "piwik-production-frontends" => ["special.verwaltung.uni-muenchen.de"],
        "piwik-test-dbs" => ["piwiktest2.verwaltung.uni-muenchen.de"],
        "piwik-test-workers"  => ["piwiktest2.verwaltung.uni-muenchen.de"],
        "piwik-test-masters"  => ["piwiktest2.verwaltung.uni-muenchen.de"],
        "piwik-test-frontends" => ["piwiktest2.verwaltung.uni-muenchen.de"],
        "piwik-dbs:children" => ["piwik-production-dbs", "piwik-test-dbs"],
        "piwik-workers:children" => ["piwik-production-workers", "piwik-test-workers"],
        "piwik-masters:children" => ["piwik-production-masters", "piwik-test-masters"],
        "piwik-frontends:children" => ["piwik-production-frontends", "piwik-test-frontends"],
        "piwik:children" => ["piwik-dbs", "piwik-masters", "piwik-workers"],
      }
      #ansible.verbose = "vvvv"
      ansible.verbose = ""
      #ansible.start_at_task = "Install Piwik"
      ansible.limit = "all"
      #ansible.tags = ["setup", "configuration", "update"]
      #ansible.skip_tags = ["update"]
      #ansible.playbook = "lmu.ansible.playbooks/base-preseed.yml"
      ansible.playbook = "lmu.ansible.playbooks/piwik.yml"
      #ansible.ask_vault_pass = true
    end
  end
end
