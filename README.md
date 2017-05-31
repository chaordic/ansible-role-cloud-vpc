cloud-vpc
=========

[![Project Status: Concept - initial development is in progress, but there has not yet been a stable, usable release suitable for the public.](http://www.repostatus.org/badges/latest/wip.svg)](http://www.repostatus.org/#wip)

Ansible role to manage Cloud VPC with different providers.

Roadmap for Cloud Providers:
* AWS - Amazon Web Services
* GCE - Google Cloud

Main roles to manage:
* Facts
* VPC
* Subnets
* Internet Gateway
* Route tables


Requirements
------------

* AWS

Boto package is required.

* GCE

> TODO

Role Variables
--------------

> TODO

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

Apache-2

Author Information
------------------

[Marco Tulio R Braga](https://github.com/mtulio)
