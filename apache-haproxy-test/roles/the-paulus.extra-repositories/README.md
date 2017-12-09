Extra Repositories
==================

Adds the EPEL, IUS, and Remi repositories to CentOS or RedHat.

Requirements
------------

RedHat or CentOS versions 6 or 7.

Role Variables
--------------

```
repository_install_epel: True
repository_install_ius: False
repository_install_remi: True
```

Dependencies
------------

None.

Example Playbook
----------------

```
    - hosts: servers
      roles:
         - { role: extra_repositories }
```

License
-------

BSD
