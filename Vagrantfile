# -*- mode: ruby -*-
# vi: set ft=ruby :

require "vagrant"

if Vagrant::VERSION < "1.2.1"
  raise "Use a newer version of Vagrant (1.2.1+)"
end

# This allows you to use environment variables to set the box ... 
# useful for a different OS / Provider.

BOX_NAME = ENV['BOX_NAME'] || "precise64"
BOX_URI = ENV['BOX_URI'] || "https://opscode-vm.s3.amazonaws.com/vagrant/boxes/opscode-ubuntu-12.04.box"

# We'll mount the Chef::Config[:file_cache_path] so it persists between
# Vagrant VMs
host_cache_path = File.expand_path("../.cache", __FILE__)
guest_cache_path = "/tmp/vagrant-cache"

Vagrant.configure("2") do |config|

    # Enable the berkshelf-vagrant plugin
    config.berkshelf.enabled = true
    # The path to the Berksfile to use with Vagrant Berkshelf
    config.berkshelf.berksfile_path = "./Berksfile"
    # Ensure Chef is installed for provisioning
    config.omnibus.chef_version = :latest


  config.vm.define :logstash_solo do |config|
    config.vm.box = BOX_NAME
    config.vm.box_url = BOX_URI
    config.vm.hostname = "logstash"
    config.vm.network :private_network, ip: "33.33.33.10"
    config.ssh.max_tries = 40
    config.ssh.timeout   = 120
    config.ssh.forward_agent = true

    config.vm.provision :chef_solo do |chef|
      chef.provisioning_path = guest_cache_path
      chef.json = {
        elasticsearch: {
          cluster_name: "logstash_vagrant",
          min_mem: '64m',
          max_mem: '64m',
          limits: {
            nofile:  1024,
            memlock: 512
          }
        },
        logstash: {
          server: {
            xms: '128m',
            xmx: '128m',
            enable_embedded_es: false,
            elasticserver_ip: '127.0.0.1'
          },
          kibana: {
            server_name: '33.33.33.10',
            http_port: '8080'
          }
        }
      }
      chef.run_list = %w[
        minitest-handler
        apt
        java
        monit
        erlang
        git
        elasticsearch
        php::module_curl
        logstash::server
        logstash::kibana
      ]
    end

    config.vm.provision :shell, :inline => <<-SCRIPT
    # Manual scripty things go here
    SCRIPT
    config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--cpus", 2]
        vb.customize ["modifyvm", :id, "--memory", 1024] 
    end
    config.vm.provider :lxc do |lxc, override|
      # Same effect as as 'customize ["modifyvm", :id, "--memory", "1024"]' for VirtualBox
      lxc.customize 'cgroup.memory.limit_in_bytes', '1024M'
    end
  end
end

require 'berkshelf/vagrant'

Vagrant::Config.run do |config|

  config.vm.define :lucid32 do |dist_config|
    dist_config.vm.box       = 'lucid32'
    dist_config.vm.box_url   = 'http://files.vagrantup.com/lucid32.box'

    dist_config.vm.customize do |vm|
      vm.name        = 'logstash'
      vm.memory_size = 1024
    end

    dist_config.vm.network :bridged, '33.33.33.10'

    dist_config.vm.provision :chef_solo do |chef|

      chef.cookbooks_path    = [ '/tmp/logstash-cookbooks' ]
      chef.provisioning_path = '/etc/vagrant-chef'
      chef.log_level         = :debug

      chef.run_list = %w[
        minitest-handler
        apt
        java
        monit
        erlang
        git
        elasticsearch
        php::module_curl
        logstash::server
        logstash::kibana
      ]

      chef.json = {
        elasticsearch: {
          cluster_name: "logstash_vagrant",
          min_mem: '64m',
          max_mem: '64m',
          limits: {
            nofile:  1024,
            memlock: 512
          }
        },
        logstash: {
          server: {
            xms: '128m',
            xmx: '128m',
            enable_embedded_es: false,
            elasticserver_ip: '127.0.0.1'
          },
          kibana: {
            server_name: '33.33.33.10',
            http_port: '8080'
          }
        }
      }
    end
  end

  config.vm.define :lucid64 do |dist_config|
    dist_config.vm.box       = 'lucid64'
    dist_config.vm.box_url   = 'http://files.vagrantup.com/lucid64.box'

    dist_config.vm.customize do |vm|
      vm.name        = 'logstash'
      vm.memory_size = 1024
    end

    dist_config.vm.network :bridged, '33.33.33.10'

    dist_config.vm.provision :chef_solo do |chef|
      chef.cookbooks_path    = [ '/tmp/logstash-cookbooks' ]
      chef.provisioning_path = '/etc/vagrant-chef'
      chef.log_level         = :debug

      chef.run_list = %w[
        minitest-handler
        apt
        java
        monit
        erlang
        git
        elasticsearch
        php::module_curl
        logstash::server
        logstash::kibana
      ]

      chef.json = {
        elasticsearch: {
          cluster_name: "logstash_vagrant",
          min_mem: '64m',
          max_mem: '64m',
          limits: {
            nofile:  1024,
            memlock: 512
          }
        },
        logstash: {
          server: {
            xms: '128m',
            xmx: '128m',
            enable_embedded_es: false,
            elasticserver_ip: '127.0.0.1'
          },
          kibana: {
            server_name: '33.33.33.10',
            http_port: '8080'
          }
        }
      }
    end
  end

  config.vm.define :centos6_32 do |dist_config|
    dist_config.vm.box       = 'centos6_32'
    dist_config.vm.box_url   = 'http://vagrant.sensuapp.org/centos-6-i386.box'

    dist_config.vm.customize do |vm|
      vm.name        = 'logstash'
      vm.memory_size = 1024
    end

    dist_config.vm.network :bridged, '33.33.33.10'

    dist_config.vm.provision :chef_solo do |chef|
      chef.cookbooks_path    = [ '/tmp/logstash-cookbooks' ]
      chef.provisioning_path = '/etc/vagrant-chef'
      chef.log_level         = :debug

      chef.run_list = %w[
        minitest-handler
        java
        yum::epel
        erlang
        git
        elasticsearch
        php::module_curl
        logstash::server
        logstash::kibana
      ]

      chef.json = {
        elasticsearch: {
          cluster_name: "logstash_vagrant",
          min_mem: '64m',
          max_mem: '64m',
          limits: {
            nofile:  1024,
            memlock: 512
            }
        },
        logstash: {
          server: {
            xms: '128m',
            xmx: '128m',
            enable_embedded_es: false,
            elasticserver_ip: '127.0.0.1'
          }
        }
      }
    end
  end
end
