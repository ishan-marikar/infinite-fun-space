How to Control which Services are Started when Ubuntu Boots
===========================================================

As a web developer I have, for example, `postgresql` and `jetty` installed on
my laptop, but I don't want these services to start every time my laptop boots
(as they will do by default after you `apt-get install` them), I want to start
them up manually only when I want to do web development. So:

    sudo apt-get install dialog rcconf
    sudo rcconf

`rcconf` shows a menu of all the available services with their names and
descriptions, and lets you toggle which start at boot.

When a service isn't started at boot you can start it manually with
`sudo service ... start`, e.g.:

    sudo service jetty start
    sudo service postgresql start
