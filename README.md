# On My Travel - Infrastructure

The first version of the application has been set-up on an Ubuntu server hosted on a virtual server.

As a proof of concept, it was enough to install the dependencies manually. But, as we try to make the application a bit more professional, we built this repository in order to automate the application deployment.

** Disclaimer ** : This repository intents to be as up-to-date as possible BUT it is set to work on an Ubuntu server 14.04x64. Some incompatibilities may or may not occurs.

## Requirements
* [Vagrant](https://www.vagrantup.com/)
* [Ansible](http://docs.ansible.com/ansible/intro_installation.html)

## Set-up

First, open a terminal and go into the repository clone folder.
Then, start the virtual machine by executing the following commands.

    cd vagrant/
    vagrant up

It should take a moment, especially if you have never downloaded the Ubuntu package that is required.
When the process is completed, let's check that the SSH connection to the local virtual machine is allowed to ansible.

    cd ../ansible/
    ansible all -m ping -u vagrant --ask-pass

It should ask you to enter a password. Type `vagrant`(default password for the default vagrant user) and the ansible command should answer:

    127.0.0.1 | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }

### First step

The first step to set the vagrant machine ready to host the application is to start with this first playbook. It creates the ansible user and secure the SSH connection.

    ansible-playbook vagrant_to_prod.yml --ask-pass