This project provides a simple sandbox environment for provisioning and testing Riak and Riak TS nodes and clusters. This project includes a submodule that links to Basho's ansible-riak role, as well as example playbooks that are used to provision and cluster Vagrant VMs. The role and playbooks can be used as working examples to provision and cluster riak nodes outside of the sandbox environment. An overview of running this material on your own machines can be found [here](https://github.com/basho-labs/riak-demos/wiki/Provision-a-cluster-outside-of-vagrant).

The full guide to running the TS demo can be found [here](http://info.basho.com/rs/721-DGT-611/images/Getting-Started-with-Riak-TS.pdf).

# What to expect
By default, this demo will:
* provision 3 VMs running Ubuntu 14.04, each with 2GB RAM and 2 CPU
* Install Riak TS via the ansible-riak ansible role
* Cluster Riak TS on all VMs
* Configure Riak Shell on all nodes for use with the cluster
* Set up a demo in `/home/vagrant/ts-demo` that includes:
  * A python notebook for creating Time Series tables
  * ~1 million rows of real world Time Series data and a loader script
  * A python notebook to query and manipulate data within Riak TS
* Launch the jupyter webserver in the demo directory to run the python notebooks


# Dependencies
* [git](https://git-scm.com/downloads) (Tested with 1.9.1)
* [ansible](http://docs.ansible.com/ansible/intro_installation.html) (Tested with 2.0.1.0)
* [vagrant](https://www.vagrantup.com/downloads.html) (Tested with 1.8.1)
* [virtualBox](https://www.virtualbox.org/wiki/Downloads) (Tested with 5.0.16)

# Quick Start
```
## Clone the repo
git clone https://github.com:basho-labs/riak-demos.git

## Go into the repo directory
cd riak-demos

## Initialize and pull down the ansible-riak role
git submodule init
git submodule update

## Spin up the demo cluster
vagrant up

## Log in to the primary node
vagrant ssh riak-ts1

## Once you're done, clean up
vagrant destroy -f
```

## Optional Variables
Vagrant and ansible settings can be configured via environment variables before a run. This demo environment is configured with sane defaults, so changing these variables is not recommended unless you really need to.
```
## Change the playbook used to install and configure the VMs
## The machines will be named after the playbook.
## In this case "riak1", "riak2", etc. Defaults to 'riak-ts.yml'
export PLAYBOOKS='riak.yml'

## Change the number of VMs provisioned. Defaults to '3'
export CLUSTER_SIZE=7

## Change CPU and RAM VMs are provisioned with
export VAGRANT_RAM=4096 #Defaults to 2048
export VAGRANT_CPU=4 #Defaults to 2

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
