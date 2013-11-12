You can configure `sudo` to allow certain users to run certain commands without
having to type a password. For example, this is my `/etc/sudoers.d/ckan` file:

    seanh ALL = (ALL) NOPASSWD:\
    /usr/sbin/service postgresql start, \
    /usr/sbin/service postgresql stop, \
    /usr/sbin/service postgresql restart, \
    /usr/sbin/service jetty start, \
    /usr/sbin/service jetty stop, \
    /usr/sbin/service jetty restart, \
    /usr/bin/psql ckan_default, \
    /usr/bin/psql ckan_test, \
    /usr/bin/psql ckan_pdeu, \
    /usr/bin/psql datastore_default, \
    /usr/bin/psql datastore_test

The permissions of the file need to be `-r--r----- 1 root root`.

Beware that if there's a syntax error in a sudoers.d file, sudo will break and
you won't be able to sudo anything. If that happens you can run use `pkexec` to
fix the file, e.g.: `pkexec vim /etc/sudoers.d/ckan`.
