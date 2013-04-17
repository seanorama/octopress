---
layout: post
title: "Blogging with Continuous Integration"
date: 2013-04-16 11:29
comments: true
Author: Sean Roberts
published: false
categories: 
---

I've recently moved to our International Product team, after 6 years of focus in Support & Support Ops at Rackspace.

Having spent so much time focused on Rackspace processes and how to do things on metal (aka dedicated servers), I had become outdated on the ways of the Cloud and the blurring of lines between development and operations _(OMG devops buzzword everywhere!)_.

With that change I took on many personal challenges to bring my knowledge back up to snuff.

One part of that, is better understanding Continuous Integration.
To help me [Just Start Writing](/blog/2013/04/16/just-start/) I built this blog. Let the documentation begin!


### Complicated start for a simple finish. The key players:
 - Blogging framework: [Octopress](http://octopress.org). It is a static site generator written in Ruby _(give it a week before I switch to Python or node.js)_.
 - Manage the code: [GitHub](http://github.com)
 - Serve the content: [Rackspace Cloud Files w/ Akamai CDN](http://www.rackspace.co.uk/cloud-files/) 
 - Write the posts: http://prose.io/
 - Generate and deploy the site to Cloud Files: [Travis-CI](http://travis-ci.org/) (a hosted Continuous Integration service)


### New to git revision control?
  1. Fun quick way to learn how Git is used: https://openhatch.org/missions/


### Setup GitHub
  1. https://help.github.com/articles/set-up-git
  1. (optional) https://help.github.com/articles/generating-ssh-keys


### Fork Octopress to your own repo:
 1. Create your own fork of [Octopress](https://github.com/imathis/octopress) by following all of [these instructions](https://help.github.com/articles/fork-a-repo).
 1. (optional) If using SSH keys instead of HTTPS, [do this](https://help.github.com/articles/why-is-git-always-asking-for-my-password).


#### Setup & Configure Octopress
 1. Follow the [Setup instructions](http://octopress.org/docs/setup/), but skip the 'git clone' since you already cloned your own fork.
 1. Follow the [Configuration instructions](http://octopress.org/docs/configuring/).
 1. Here's what I have changed including some extras for http://prose.io.
 <script src="https://gist.github.com/seanorama/5397222.js"></script>
 1. Create your first post & page which we will edit later in http://prose.io
   - `$ rake new_post["title"] `
   - `$ rake new_page[about]`
   - Further reference: [Blogging Basic with Octopress](http://octopress.org/docs/blogging/)
 - Commit files to GitHub
   - `git add .`
   - `git commit -m "Initial configuration"`
   - `git push origin master`

#### Prepare the container in Rackspace Cloud Files
 - Create a Cloud Files container from your Rackpace Cloud Control panel
 - CDN enable that container
 - Configure DNS: Create a CNAME which points to the CDN HTTP address created above. I went with www.recountal.org.
 - `sudo pip install python-swiftclient`
 - `export OS_USERNAME=yourRackspaceUsername`
 - `export OS_PASSWORD=yourRackspaceApikey`
 - `export OS_AUTH_URL=https://identity.api.rackspacecloud.com/v1.0`
 - Prepend 'lon.' to that domain if using Rackspace UK
 - `swift -A $OS_AUTH_URL -U $OS_USERNAME -K $OS_PASSWORD post -m "X-Container-Meta-Web-Index: index.html" myblog`


#### Configure Travis CI
 - Do steps 1 & 2 of the [http://about.travis-ci.org/docs/user/getting-started/](Travis CI Getting Started documentation)
 - Update your .travis.yml to look like this, but exclude the ' - secure' entries.
 - Add your own credentials for Travis by executing these commands (update with your githubusername/reponame, Rackspace username, Rackspace apikey)
   - travis encrypt --add -r githubusername/reponame OS_USERNAME=yourRackspaceUsername
   - travis encrypt --add -r githubusername/reponame OS_PASSWORD=yourRackspaceApikey

#### Travis CI notifications on IRC
 - TODO: Own a Freenode channel
 - Update .travis.yml with notification settings http://about.travis-ci.org/docs/user/notifications/#IRC-notification
```
11:54 -!- travis-ci [~travis-ci@ec2-50-17-148-215.compute-1.amazonaws.com] has joined ##seano
11:54 < travis-ci> [travis-ci] seanorama/octopress#28 (master - ec91eff : Sean Roberts): The build passed.
11:54 < travis-ci> [travis-ci] Change view : https://github.com/seanorama/octopress/compare/e6aab22cd442...ec91effb3c12
11:54 < travis-ci> [travis-ci] Build details : http://travis-ci.org/seanorama/octopress/builds/6412185
11:54 -!- travis-ci [~travis-ci@ec2-50-17-148-215.compute-1.amazonaws.com] has left ##seano []
```
 

#### Prose.io
 - Write posts & pages in http://prose.io/ . Nuff said.

# Do to get updates from upstream octopress 
- `git fetch upstream` # Fetches any new changes from the original repository
- `git merge upstream/master` # Merges any changes fetched into your working files



Prose:
  - Activate and edit with: http://prose.io/
  - Implement the "nice settings" from http://darvin.github.io/blog/2013/01/13/Prose_Octopress_TravisIO/

Prose: