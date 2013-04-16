---
layout: post
title: "Blogging with Continuous Integration"
date: 2013-04-16 11:29
comments: true
Author: Sean Roberts
published: false
categories: 
---

I've recently moved to our International Product team, after 6 years of focus in Support or Support Ops at Rackspace.
Having spent so much time focused on Rackspace processes and how to do things on metal (aka dedicated servers), I had become outdated on the ways of the Cloud and the blurring of lines between development and operations (OMG devops buzzaord everywhere!).

With that change I took on many personal challenges. This is one of them.

#### Static blogging with Continuous Integration to Rackspace Cloud Files
For [Just Starting a weblog](/blog/2013/04/16/just-start/) there are many blogging services. No need to deal with managing anything. Hell, I already have accounts on Tumblr, Wordpress and every other site out there.

But, to help with my learning, I decided to implement the following:
 - Blogging platform: [Octopress](http://octopress.org) (a static site generator written in Ruby. Tempted to use something with Python or node.js instead.)
 - Hold the code: [GitHub](http://github.com)
 - Deploy: [Travis-CI](http://travis-ci.org/) (A Hosted Continuous Integration service)
 - Write the posts: vim or http://prose.io/ depending on my mood and location
 - Serve the content: [Rackspace Cloud Files w/ Akamai CDN](http://www.rackspace.co.uk/cloud-files/)


#### Fork on github: 
 - Create your own fork of [Octopress](https://github.com/imathis/octopress) by following all of [these instructions](https://help.github.com/articles/fork-a-repo).
 - (optional) If using SSH keys instead of HTTPS for connecting to GitHub, [do this](https://help.github.com/articles/why-is-git-always-asking-for-my-password).

#### Setup & Configure Octopress
 - Follow the [Setup instructions](http://octopress.org/docs/setup/), but skip the 'git clone' since you already cloned your own fork.
 - Follow the [Configuration instructions](http://octopress.org/docs/configuring/).
 - Here's what I have changed including some extras for 'prose.io'.
   - (PUT A DIFF HERE FROM THE ORIGINAL IN MY REPO)
 - Create your first post & page as containers which we will edit later in http://prose.io
   - `$ rake new_post["title"] `
   - `$ rake new_page[about]`
   - More details for [Blogging Basic with Octopress](http://octopress.org/docs/blogging/)
 - Commit files to GitHub
   - git add . 
   - git commit -m "Initial configuration"
   - git push origin master

#### Prepare the container in Rackspace Cloud Files or any OpenStack Swift provider
 - Create a Cloud Files container from your Rackpace Cloud Control panel
 - CDN enable that container
 - Configure DNS: Create a CNAME which points to the CDN HTTP address created above. I went with www.recountal.org.
 - Set the index using python-swiftclient
   - (if not already installed) $ sudo pip install python-swiftclient
   - $ swift -A https://lon.identity.api.rackspacecloud.com/v1.0 -U $OS_USERNAME -K $OS_PASSWORD post -m "X-Container-Meta-Web-Index: index.html" myblog
     - The URL is for Rackspace UK. If your cloud is in the US, remove the 'lon.'
     - replace $OS_USERNAME and $OS_PASSWORD with your Rackspace username and apikey.

#### Configure Travis CI
 - Do steps 1 & 2 in the [http://about.travis-ci.org/docs/user/getting-started/](Travis CI Getting Started documentation)
 - Update your .travis.yml to look like this, but exclude the ' - secure' entries. Yours will be added in the next step.
 - Add your Rackspace username & apikey
   - travis encrypt --add -r seanorama/octopress OS_USERNAME=yourRackspaceUsername
   - travis encrypt --add -r seanorama/octopress OS_PASSWORD=yourRackspaceApikey

#### Prose.io
 - I do all of my editing from http://prose.io/ . Nuff said.




# Build after each new post:
rbenv hash
bundle install
rake install
rake generate

# Do to get updates from upstream octopress 
git fetch upstream # Fetches any new changes from the original repository
git merge upstream/master # Merges any changes fetched into your working files



Prose:
  - Activate and edit with: http://prose.io/
  - Implement the "nice settings" from http://darvin.github.io/blog/2013/01/13/Prose_Octopress_TravisIO/

Prose:
