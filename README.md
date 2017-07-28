# On My Travel - Infrastructure

The first version of the application has been set-up on an Ubuntu server hosted on a virtual server.

As a proof of concept, it was enough to install the dependencies manually. But, as we try to make the application a bit more professional, we built this repository in order to automate the application deployment.

** Disclaimer ** : This reposimarktory intents to be as up-to-date as possible BUT it is set to work on an Ubuntu server 14.04x64. Some incompatibilities may or may not occurs.

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
    ansible all -m ping -i inventories/setup -u vagrant --ask-pass

Where `all` is the group of machine we want to ping (all in this case) and `vagrant` is the ssh username.

It should ask you to enter a password. Type `vagrant` (default password for the default vagrant user) and the ansible command should answer:

    127.0.0.1 | SUCCESS => {
        "changed": false,
        "ping": "pong"
    }

### First step

The first step to set the vagrant machine ready to host the application is to start with this first playbook. It creates the ansible user and secure the SSH connection.

But first, it's necessary to generate an SSH key that you will use to authenticate yourself on the server. **This step is optional if you already have an id_rsa keyfile.**

As usual, to generate your SSH key:

    ssh-keygen -t rsa -b 4096 -C "<YOUR EMAIL HERE>"

It will ask you to select the directory where you want to generate the SSH key. It is usually created as `/Users/<YOU>/.ssh/id_rsa`. You can either use this one or change the SSH keypath in the project.

Then, it's simple:

    ansible-playbook -i inventories/setup setup.yml --ask-pass

From now, you should be able to connect on the virtual machine using the following command. The IP is set in the vagrant file to `172.28.128.3` and the default user is `ansible`. If you changed things, do not hesitate to update it accordingly.

    ssh ansible@172.28.128.3 -p 2222

If everything goes OK, then you can run the next command. Obviously, it is important to pick the right inventory (local for vagrant, master for the server).

    ansible-playbook -i inventories/local install.yml
