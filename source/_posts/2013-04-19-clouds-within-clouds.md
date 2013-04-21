---
layout: post
title: "Clouds within Clouds: Rackspace Private Cloud within Public Cloud!"
comments: true
categories: [education, rackspace, openstack]
published: false

---

# Clouds within Clouds: Rackspace Private Cloud within Public Cloud! Part 1.

I wanted to play with our, Rackspace, Private Cloud. At 1st I got some metal (physical servers) but dealing with them was a pain and I kept wanting more.

Then I discovered that the infrastructure can existing on top of a Cloud Infrastructure. Less headache than lining hardware up, and with very little cost!

For their own testing, the Private Cloud folks made this:

It will look like this:
- Several Rackspace Cloud Servers connected with Cloud Networks
- OpenCenter installed to create & manage the Private Cloud Nova Cluster

And the Private Cloud guys have [made it even easier](https://github.com/rcbops/opencenter-install-scripts) by providing scripts which will get the base infrastructure built for you within a Rackspace Public Cloud account _(though you could modify the scripts to work with other providers)_.

The scripts will:
 - Create the Cloud Server instances which are needed.
 - Deploy OpenCenter on to those instances.

With that, you have a clean OpenCenter installation. From there you are ready to begin provisioning your own Private Cloud.

Lets dive in.

#### 1. What you need

1. A Rackspace Public Cloud account
2. Money to pay for the Cloud Server instances & Cloud Networks we are about to create
3. Somewhere to execute these scripts (which I'll call your 'workstation') and these [prerequisites fulfilled](https://github.com/rcbops/opencenter-install-scripts#prerequisities-for-installing-opencenter-cluster).
  - TODO: should submit some updates to that documentation to give links to configuring Nova and other stuff. That is outside the scope of this document.


#### 2. (optional) Create your workstation
Don't have somewhere to run the scripts from? Then do this:

1. New Ubuntu 12.10 instance in your cloud provider of choice (Rackspace for me).
2. Install python-pip & unzip: `sudo apt-get update; sudo apt-get install python-pip unzip`
3. Install rackspace-novaclient: `sudo pip install rackspace-novaclient`
4. Configure your Rackspace credentials for the Nova client
http://www.rackspace.com/knowledge_center/article/installing-python-novaclient-on-linux-and-mac-os
3. Configure nova for authentication. See the 'Environment Variables' section [here](http://www.rackspace.com/knowledge_center/article/installing-python-novaclient-on-linux-and-mac-os)
4. Put those changes in place: `source ~/.bash_profile`
5. Create your SSH key (with no passphrase): `mkdir ~/.ssh; chmod 700 ~/.ssh; ssh-keygen -t rsa`


#### 3. Get 'opencenter-install-scripts'

1. Using 'wget' & 'unzip' here, but feel free to use 'git':
```
wget https://github.com/rcbops/opencenter-install-scripts/archive/master.zip
unzip master.zip
```

#### 4. Read the documentation fool!

1. `cd opencenter-install-scripts-master`
2. `./opencenter-cluster.sh -h`
3. You'll see something like this:

```
usage: ./opencenter-cluster.sh options

This script will install an OpenCenter Cluster.

OPTIONS:
  -h --help  Show this message
  -v --verbose  Verbose output
  -V --version  Output the version of this script

ARGUMENTS:
  -p --prefix=<Cluster Prefix>
         Specify the name prefix for the cluster - default "c1"
  -s --suffix=<Cluster Suffix>
         Specify a cluster suffix - default ".opencenter.com"
         Specifying "None" will use short name, e.g. just <Prefix>-opencenter-sever
  -c --clients=<Number of Clients>
         Specify the number of clients to install, in conjunction with a server & dashboard - default 2
  -pass --password=<Opencenter Server Password>
         Specify the Opencenter Server Password - only used for package installs - default "opencenter"
  -pkg --packages
         Install using packages
  -a --add-clients
         Add clients to Opencenter Cluster specified by Prefix
         Can't be used in conjunction with --rerun/-rr
         NB - If password was used for original cluster, password must be the same as existing cluster's password
  -n --network=<CIDR>|<Existing network name>|<Existing network uuid>
         Setup a private cloud networks, will require "nova network-create" command - default 192.168.0.0/24
         You can specify an existing network name or network uuid
  -o --os=[redhat | centos | ubuntu | fedora ]
         Specify the OS to install on the servers - default ubuntu
  -pk --public-key=[location of key file]
         Specify the location of the key file to inject onto the cloud servers
  -rr --rerun
         Re-run the install scripts on the servers, rather than spin up new servers
         Can't be used in conjunction with --add-clients/-a
```

#### Build the infrastructure! An OpenCenter Cluster.

1. Check out my sad empty Cloud Control Panel :(
![Cloud Control Panel before OpenCenter](/images/Selection_043.bmp)
2. Build the cluster. If you read the documentation above, then you know what it's doing!: `./opencenter-cluster.sh  --suffix=.seanroberts.org --password=YourCrazyPasswordHere --public-key=~/.ssh/id_rsa.pub --clients=5`
3. Wait. This will take a while. I'm told faster Cloud Server builds are coming soon.
4. While you are waiting, go check out your Control Panel. Here's mine:
![Cloud Control Panel during OpenCenter](/images/Selection_043.png)
5. When it's done, you'll see this output from the script:
```
*** COMPLETE ***

Run "export OPENCENTER_ENDPOINT=http://Y.Y.Y.Y:8080" to use the opencentercli
Or connect to "http://X.X.X.X:3000" to manage via the opencenter-dashboard interface
```
6. 
![Cloud Control Panel during OpenCenter](/images/Selection_044.png)


#### Build your Private Cloud with OpenCenter

1. Open your web browser to the dashboard URL which the script gave you (the one ended in :3000). Welcome to the Rackspace Private Cloud Control Panel:
![Cloud Control Panel during OpenCenter](/images/Selection_046.png)
2. What to do next is [documented in the Rackspace Knowledge Center](http://www.rackspace.com/knowledge_center/article/installing-rackspace-private-cloud-software#access-the-gui). But it's not the best documentation.
3. So lets do this with pretty screenshots.


3. Make the 1st node a Chef server. Read more about it [here](http://www.rackspace.com/knowledge_center/article/installing-rackspace-private-cloud-software#install-chef-server):
![Cloud Control Panel during OpenCenter](/images/Selection_045.png)

{% img [class names] /path/to/image [width] [height] [title text [alt text]] %}

4. Now the fun starts. Lets [Create your Nova Cluster](
http://www.rackspace.com/knowledge_center/article/installing-rackspace-private-cloud-software#install-nova-cluster)

Again 2 clicks:
![Create Nova Cluster](/images/Selection_048.png)

Then you get this:
![Additional Input Required](/images/Selection_048.png)






While you are waiting, go check
3. Profit?

#### That's it?

Yep. The script goes off and builds your infrastructure!






Workstation requirements:
 - git
 - python
 - [pyrax](https://github.com/rackspace/pyrax#readme)
 - no

 
 pip
sudo apt-get update
sudo apt-get install python-pip
 From a fresh Rackspace Cloud Server with Ubuntu 12.10:
 -