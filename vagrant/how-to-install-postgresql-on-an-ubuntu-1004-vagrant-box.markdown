How to Install PostgreSQL on an Ubuntu 10.04 Vagrant Box
========================================================

Here's how to install PostgreSQL and have it run automatically at startup, on
an Ubuntu 10.04 virtual machine using [Vagrant](http://vagrantup.com/). This
took me a while to figure out:

Add the default lucid32 base box to your vagrant, if you haven't already:

    host> vagrant box add lucid32 http://files.vagrantup.com/lucid32.box 

Now make a new lucid32 virtual machine and install postgresql on it:

    host> mkdir mybox
    host> cd mybox
    host> vagrant init lucid32
    host> vagrant up
    host> vagrant ssh
    vagrant@lucid32> sudo aptitude update 
    vagrant@lucid32> sudo aptitude install postgresql

Now you need to fix your locale settings (thanks, [Crowd Interactive Tech
Blog](http://blog.crowdint.com/2011/08/11/postgresql-in-vagrant.html)). Edit
`/etc/bash.bashrc`:

    vagrant@lucid32> sudo nano /etc/bash.bashrc

        # Add these lines to the bottom of the file:
        export LANGUAGE=en_US.UTF-8
        export LANG=en_US.UTF-8
        export LC_ALL=en_US.UTF-8

    vagrant@lucid32> sudo locale-gen en_US.UTF-8
    vagrant@lucid32> sudo dpkg-reconfigure locales

Disconnect from your virtual machine (ctrl-d) then connect again:

    host> vagrant ssh
    vagrant@lucid32> sudo pg_createcluster 8.4 main --start
    vagrant@lucid32> sudo su
    root@lucid32> sudo -u postgres psql -l

You should see the list of databases output from psql.

Now so you don't have to do it all again, disconnect from your virtual machine
again and package it:

    host> vagrant halt
    host> vagrant package
    host> vagrant box add lucid32_with_postgresql package.box
