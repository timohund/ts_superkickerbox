Introduction
============

This is an example cookbook to show how to create a dev environment
for a php based symfony2 app with "ts_basebox" and "ts_phpapp".


It installs the "master" branch of "https://github.com/timoschmidt/superkicker.git"
which is a symfony2 based soccer tipp game.

Before you start
---------------

Install Vagrant:

http://www.vagrantup.com/downloads.html

Install Chef SDK:

http://getchef.com/downloads/chef-dk

Install Berkshelf, Linrarian Chef, Omnibus and Cachier plugin:

vagrant plugin install vagrant-berkshelf
vagrant plugin install vagrant-librarian-chef
vagrant plugin install vagrant-omnibus
vagrant plugin install vagrant-cachier

Ready to go?!
----------------

Run:
vagrant up

Add to your /etc/hosts :
192.168.33.33 superkicker.devbox.local

Open:
http://superkicker.devbox.local/app_dev.php

And see a running php based symfony app.