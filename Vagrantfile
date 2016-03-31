# -*- mode: ruby -*-
# vi: set ft=ruby :


require 'json'
Vagrant.configure('2') do |config|
  config.ssh.forward_agent = true
  ## Grab a list of roles to run, or default to a basic set
  roles = (ENV['PLAYBOOKS'] || 'riak-ts.yml').gsub(/\.yml/,'').split(',') 

  ## Allow multiple machines to be provisioned per role
  cluster_size = (ENV['CLUSTER_SIZE'] || 3)

  ## Set memory size and # of cpus from env vars
  config.vm.provider "virtualbox" do |v|
    v.memory = (ENV['VAGRANT_RAM'] || 2048)
    v.cpus = (ENV['VAGRANT_CPU'] || 2)
  end

  ## Set ansible 'extra_vars' and 'tags' from env vars
  extra_vars = Hash.new
  extra_vars = JSON.parse(ENV['EXTRA_VARS']) unless ENV['EXTRA_VARS'].nil?
  extra_vars['ansible_ssh_user'] = 'vagrant'
  tags = (ENV['TAGS'] || 'all').split(',')

  ## Allow ansible to run on all machines when a playbook is run
  limit = (ENV['LIMIT'] || 'all')

  ## Generate boxes for each role we feed in!
  i = 0
  roles.each do |role|
    ## Construct an array of all hostnames that will be associated
    ## with the role so we can loop over it to build the vms and
    ## feed the array to ansible.groups so it builds the inventory
    ## correctly
    vm_names = Array.new
    (1..cluster_size).each do |count|
      vm_names.push("#{role}#{count}")
    end

    ## Define and spin up the machines
    vm_names.each do |vm_name|
      config.vm.define vm_name do |rolevm|
        rolevm.vm.hostname = vm_name
        rolevm.vm.box = ENV['BOX'] || 'ubuntu/trusty64'
        rolevm.vm.network 'private_network', ip: "172.17.8.#{i+100}"
        i += 1

        ## If this is the last server in the group, or limit is not
        ## set to 'all', provision the machines. This will make the
        ## playbooks run serially if a limit is not defined, or if it is,
        ## will run playbooks in parallel across all hosts in the group.
        config.ssh.insert_key = false if limit == 'all'
        if vm_name == vm_names[-1] || limit != 'all'
          rolevm.vm.provision :ansible do |ansible|
            ansible.groups = {
              role => vm_names
            }
            ansible.extra_vars = extra_vars
            ansible.tags = tags 
            ansible.playbook = "ansible/#{role}.yml"
            ansible.limit = 'all' if limit == 'all'
          end
        end
      end
    end
  end
end
