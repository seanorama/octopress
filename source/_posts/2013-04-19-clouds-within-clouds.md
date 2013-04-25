---
layout: post
title: "Clouds within Clouds: Rackspace Private Cloud within Public Cloud!"
comments: true
categories: [education, rackspace, openstack]
published: false

---

DRAFT!



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
3. Install rackspace-novaclient: `sudo pip install python-novaclient`
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
rm master.zip
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
![Cloud Control Panel before OpenCenter](http://i.imgur.com/a42HrzN.png)
2. Build the cluster. If you read the documentation above, then you know what it's doing!: `./opencenter-cluster.sh  --suffix=.seanroberts.org --password=YourCrazyPasswordHere --network=192.168.0.0/24 --public-key=~/.ssh/id_rsa.pub --clients=5`
3. Wait. This will take a while. I'm told faster Cloud Server builds are coming soon.
4. While you are waiting, go check out your Control Panel. Here's mine:
![Cloud Control Panel during OpenCenter](http://i.imgur.com/2uSQxZK.png)
5. When it's done, you'll see this output from the script:

```
*** COMPLETE ***

Run "export OPENCENTER_ENDPOINT=http://Y.Y.Y.Y:8080" to use the opencentercli
Or connect to "http://X.X.X.X:3000" to manage via the opencenter-dashboard interface
```

6. And this in your Cloud Control Panel:
![Cloud Control Panel during OpenCenter](http://i.imgur.com/yyE96Dy.png)

#### Quick note: SSH to your 

If you want to access any of the servers, simply SSH to it's IP with the user root. Your key is there so you'll login automatically. 

```
$ ssh root@999.999.999.999
Welcome to Ubuntu 12.04.2 LTS (GNU/Linux 3.2.0-38-virtual x86_64)
... text snipped ...
root@c1-opencenter-server:~#
```

#### Build your Private Cloud with OpenCenter

1. Open your web browser to the dashboard URL which the script gave you (the one ended in :3000). Welcome to the Rackspace Private Cloud Control Panel:
![Cloud Control Panel during OpenCenter](http://i.imgur.com/cczeJKT.png)
2. What to do next is [documented in the Rackspace Knowledge Center](http://www.rackspace.com/knowledge_center/article/installing-rackspace-private-cloud-software#access-the-gui). But it's not the best documentation.
3. So lets do this with pretty screenshots.


3. Make the 1st node a Chef server. Read more about it [here](http://www.rackspace.com/knowledge_center/article/installing-rackspace-private-cloud-software#install-chef-server):
![Cloud Control Panel during OpenCenter](http://i.imgur.com/4waIdr7.png)

4. Now the fun starts. Lets [Create your Nova Cluster](
http://www.rackspace.com/knowledge_center/article/installing-rackspace-private-cloud-software#install-nova-cluster)

Step 1:
* ![Create Nova Cluster](http://i.imgur.com/rVA6O6S.png)

Step2:
* Then you get this which needs better documentation:
![Additional Input Required](http://i.imgur.com/2pSzfcE.png)

* TODO: Document why I chose each:
![Additional Input Required](http://i.imgur.com/hPTZ9Aq.png)

Step3:
![Step 3](http://i.imgur.com/CyF5Z6R.png)

Step4:
![Step 4](http://i.imgur.com/9sPAT7F.png)



[Upload Glance Images](http://www.rackspace.com/knowledge_center/article/installing-rackspace-private-cloud-software#nova-cluster-images)

![Imgur](http://i.imgur.com/r9GqCxV.png)

#### Openstack installed

It's done. OpenCenter did it's job. Now what? It's not clear from any of the documentation. Here's a start:
1. In the OpenCenter Dashboard, note the name of your Infrastructure Node (from 'Step 3' above)
2. Get it's 'ServiceNet' IP from your Rackspace Public Cloud Control Panel
3. SSH: `ssh root@thehostname`
4. Load your credentials for Nova: `source .novarc`

#### Creating an instance in your Cloud!

http://www.rackspace.com/knowledge_center/article/rackspace-private-cloud-software-creating-an-instance-in-the-cloud

1. Generate an SSH Keypair. Save the output as 'admin-keypair.pem': `nova keypair-add admin-keypair`
2. Update the Default Security Group
```
$ nova secgroup-add-rule default tcp 22 22 0.0.0.0/0
$ nova secgroup-add-rule default icmp -1 -1 0.0.0.0/0
```
3. Create your first server instance!
```
# nova boot --key-name admin-keypair --image "precise" --flavor 1 n1
+-------------------------------------+--------------------------------------+
| Property                            | Value                                |
+-------------------------------------+--------------------------------------+
| OS-DCF:diskConfig                   | MANUAL                               |
| OS-EXT-SRV-ATTR:host                | None                                 |
| OS-EXT-SRV-ATTR:hypervisor_hostname | None                                 |
| OS-EXT-SRV-ATTR:instance_name       | instance-00000005                    |
| OS-EXT-STS:power_state              | 0                                    |
| OS-EXT-STS:task_state               | scheduling                           |
| OS-EXT-STS:vm_state                 | building                             |
| accessIPv4                          |                                      |
| accessIPv6                          |                                      |
| adminPass                           | ywc5DjSD8JQj                         |
| config_drive                        |                                      |
| created                             | 2013-04-22T13:15:33Z                 |
| flavor                              | m1.tiny                              |
| hostId                              |                                      |
| id                                  | 2e402876-2cd2-46e2-87fa-43d2f0837e97 |
| image                               | precise                              |
| key_name                            | admin-keypair                        |
| metadata                            | {}                                   |
| name                                | n1                                   |
| progress                            | 0                                    |
| security_groups                     | [{u'name': u'default'}]              |
| status                              | BUILD                                |
| tenant_id                           | 70b55e0f65554df9b83f8c7f71a78df4     |
| updated                             | 2013-04-22T13:15:34Z                 |
| user_id                             | 3804247c053749e8b305abfe7a028d4d     |
+-------------------------------------+--------------------------------------+

After a few minutes, you should be able to login directly with SSH or using:
`# nova ssh n1 -i admin-keypair.pem`