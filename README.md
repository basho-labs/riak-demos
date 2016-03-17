This project provides a simple sandbox environment for provisioning and testing Riak and Riak TS nodes and clusters. This project includes a submodule that links to Basho's ansible-riak role, as well as example playbooks that are used to provision and cluster Vagrant VMs. The role and playbooks can be used as working examples to provision and cluster riak nodes outside of the sandbox environment.

# Dependencies
* ansible (Tested with 2.0.1.0)
* vagrant (Tested with 1.8.1)
* VirtualBox (Tested with 5.0.16)

# Quick Start
```
## Clone the repo
git clone git@github.com:basho-labs/riak-demos

## Initialize and pull down the ansible-riak role
git submodule init
git submodule update

## Spin up the demo cluster
vagrant up

## Log in to the primary node
vagrant ssh riak1
```

## Optional Variables
Vagrant and ansible settings can be configured via environment variables before a run. This demo environment is configured with sane defaults, so changing these variables is not recommended unless you really need to.
```
## Change the playbook used to install and configure the VMs
## The machines will be named after the playbook.
## In this case "riak-ts1", "riak-ts2", etc. Defaults to 'riak.yml'
export PLAYBOOKS='riak-ts.yml'

## Change the number of VMs provisioned. Defaults to '5'
export ROLE_NUM=7

## Change CPU and RAM VMs are provisioned with
export VAGRANT_RAM=4096 #Defaults to 1024
export VAGRANT_CPU=4 #Defaults to 1

## Change the OS vagrant will provision
## Defaults to 'ubuntu/trusty64', has not been tested with others
export BOXES='centos/7'

## Change variables for an ansible playbook run
## This must be a json string
export EXTRA_VARS='{"ring_leader_ip": "10.34.27.97", "riak_ring_size": 128}'

## Run ansible playbooks serially on each VM. By default, ansible will
## run each playbook in parallel across many hosts. While running serially
## will take much longer and is not recommended, it can be useful for debugging.
export LIMIT='single'
```
