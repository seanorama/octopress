---
layout: post
title: "Your own Private Cloud on Public Cloud: Part 1"
comments: true
categories: 
published: false

---


### Want:
- My own Private Cloud to play with
- Provision it quickly and cheaply
- Manage it easily

### Want fulfilled:

**Built on top of another Cloud**, no metal (physical servers) to mess about with
  - Rackspace Public Cloud in my case, but this should work with most OpenStacks.
- [Rackspace Private Cloud with OpenCenter](http://www.rackspace.com/cloud/private/openstack_software/?uk):
> OpenCenter for Rackspace Private Cloud Software provides a centralized graphical user interface and API that simplifies the operational activities associated with deploying, managing, and maintaining private clouds powered by OpenStack®.

### What you need:
- 1. An OpenStack Cloud to build the infrastructure on. Such as Rackspace Public Cloud
- 2. Your workstation which meets [these prerequisites](https://github.com/rcbops/opencenter-install-scripts#prerequisities-for-installing-opencenter-cluster).

#### (optional) Create a workstation
Don't have somewhere to run the scripts from? Then do this:

1. New Ubuntu 12.10 instance in your cloud provider of choice (Rackspace for me).
2. Install python-pip & unzip, python-novaclient:
`sudo apt-get update; sudo apt-get install python-pip unzip`
`sudo pip install python-novaclient`
3. Create your SSH key (with no passphrase)
`mkdir ~/.ssh; chmod 700 ~/.ssh; ssh-keygen -t rsa` # Don't set a passphrase

4. Configure your Rackspace credentials for the Nova client
http://www.rackspace.com/knowledge_center/article/installing-python-novaclient-on-linux-and-mac-os
3. Configure nova for authentication. See the 'Environment Variables' section [here](http://www.rackspace.com/knowledge_center/article/installing-python-novaclient-on-linux-and-mac-os)
4. Put those changes in place: `source ~/.bash_profile`
5. Create your SSH key (with no passphrase): ``



1. Get the OpenCenter Cluster Installer
 - Builds your infrastructure (i.e. Cloudserver instances in Rackspace Public Cloud or your Openstack of choice)
```
wget https://github.com/rcbops/opencenter-install-scripts/archive/master.zip
unzip master.zip
rm master.zip
```

2. Get the OpenCenter CLI.
```
wget https://github.com/rcbops/opencenter-client/archive/master.zip
unzip master.zip
cd opencenter-client-master
sudo python setup.py install
rm master.zip
```

3. Build your infrastructure:
./opencenter-cluster.sh --suffix=.seanroberts.org --password=crazypasswordhere --packages --public-key=~/.ssh/id_rsa.pub --clients=4

That will run for a while. Go for a walk. Damn slow build times :(

3. In the end, you'll have this:
```
*** COMPLETE ***

Run "export OPENCENTER_ENDPOINT=https://admin:crazypasswordhere@X.X.X.X:8443" to use the opencentercli
Or connect to "https://Y.Y.Y.Y:443" to manage via the opencenter-dashboard interface
```



5. With that, you can begin to build your PrivateCloud following this guide:
http://www.rackspace.com/knowledge_center/article/using-the-opencenter-cli






Tip: Check on your server instances.

From novaclient:
$ nova list --name '.*opencenter.*'

From Rackspace Public Cloud Control Panel:
