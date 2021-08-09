
# Ansible controller and agent nodes set up guide
- Clone this repo and run `vagrant up`
- `(double check syntax/intendation)`

## We will use 18.04 ubuntu for ansible controller and agent nodes set up 
### Please ensure to refer back to your vagrant documentation

- **You may need to reinstall plugins or dependencies required depending on the OS you are using.**

```vagrant 
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what

# MULTI SERVER/VMs environment 
#
Vagrant.configure("2") do |config|

# creating first VM called web  
  config.vm.define "web" do |web|
    
    web.vm.box = "bento/ubuntu-18.04"
   # downloading ubuntu 18.04 image

    web.vm.hostname = 'web'
    # assigning host name to the VM
    
    web.vm.network :private_network, ip: "192.168.33.10"
    #   assigning private IP
    
    config.hostsupdater.aliases = ["development.web"]
    # creating a link called development.web so we can access web page with this link instread of an IP   
        
  end
  
# creating second VM called db
  config.vm.define "db" do |db|
    
    db.vm.box = "bento/ubuntu-18.04"
    
    db.vm.hostname = 'db'
    
    db.vm.network :private_network, ip: "192.168.33.11"
    
    config.hostsupdater.aliases = ["development.db"]     
  end

 # creating are Ansible controller
  config.vm.define "controller" do |controller|
    
    controller.vm.box = "bento/ubuntu-18.04"
    
    controller.vm.hostname = 'controller'
    
    controller.vm.network :private_network, ip: "192.168.33.12"
    
    config.hostsupdater.aliases = ["development.controller"] 
    
  end

end
```
![diagram](diagram-ansible.png)

- To add a machine to the list of hosts you direct to /etc/ansible/hosts start with IP, type of connection, username and password: `192.168.33.10 ansible_connection=ssh ansible_ssh_user=vagrant ansible_ssh_pass=`
- To ping a host you can use `ansible all -m ping` replace `all` with the name of the machines
- To execute `uname` in all of the machines in the hosts file you can do `ansible all -a "uname -a"`
- To check dates in location of all machines `ansible web -a "date"`
- To check memory status `ansible all -a "free"`
- To check uptime of all servers `ansible all -a "uptime"` or `ansible multi -a uptime` 
- To reboot the server `ansible all -m reboot -a reboot_timeout=3600 -u vagrant -i ansible_hosts -b` 
```Sh
Here

-m – represents the module

-a – additional parameter to the reboot module to set the timeout to 3600 seconds

-u – remote SSH user

-i – inventory file

-b – to instruct ansible to become root user before executing the task

Here is the execution output of this ad hoc command. we have three commands in this screenshot, First is to check the status and second to reboot and third one is to check the uptime
```